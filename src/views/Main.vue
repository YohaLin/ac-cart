<template>
  <div>
    <!-- Modal ，希望在在currentStep的時候秀出來-->
    <div id="modalBackground" v-show="currentStep === 4"></div>
    <div id="modalPage" v-show="currentStep === 4">
      <div id="modalPageContainer">
        <p>salutation: "{{ form.salutation }}",</p>
        <p>username: "{{ form.username }}",</p>
        <p>phone: "{{ form.phone }}",</p>
        <p>email: "{{ form.email }}",</p>
        <p>city: "{{ form.city }}",</p>
        <p>addr: "{{ form.addr }}",</p>
        <p>deliveryFee: "{{ form.deliveryFee }}",</p>
        <p>ccname: "{{ form.ccname }}",</p>
        <p>cardnumber: "{{ form.cardnumber }}",</p>
        <p>expdate: "{{ form.expdate }}",</p>
        <p>cvv: "{{ form.cvv }}",</p>
        <p>totalPrice: "{{ total }}",</p>
        <div class="modalPageButton d-flex justify-content-between">
          <button
            type="button"
            class="btn btn-secondary"
            @click.stop.prevent="clickModalPageButton"
          >
            取消
          </button>
          <button
            type="button"
            class="btn btn-primary"
            @click.stop.prevent="clickModalPageButton"
          >
            確定訂單正確
          </button>
        </div>
      </div>
    </div>
    <!-- main -->
    <main id="main" class="main-panel">
      <div class="container main-container">
        <h1>結帳</h1>
        <div class="main-content">
          <!--       左邊內容       -->
          <div class="left-content">
            <!--    進度條，放入component    -->
            <Stepper :currentStep="currentStep" />

            <!--    輸入表格，放入component     -->
            <Form
              :currentStep="currentStep"
              :initialForm="form"
              :total="total"
              @after-add-delivery-fee="afterAddDeliveryFee"
              @after-no-delivery-fee="afterNoDeliveryFee"
              @after-form-change="afterFormChange"
            />

            <!--       上下一步      -->
            <hr class="seperate-line" />
            <StepControl
              :steps="steps"
              :currentStep="currentStep"
              :total="total"
              :form="form"
              @after-add-step="afterAddStep"
              @after-minus-step="afterMinusStep"
              @after-finish-form="afterFinishForm"
            />
          </div>

          <!--       右邊內容，放入component        -->
          <div class="right-content">
            <!--       購物車，放入component       -->
            <Cart
              :initialCarts="carts"
              :total="total"
              @after-add-quantity="afterAddQuantity"
              @after-delete-quantity="afterDeleteQuantity"
            />
          </div>
        </div>
      </div>
    </main>
  </div>
</template>

<script>
import Stepper from "./../components/Stepper";
import Form from "./../components/Form";
import StepControl from "./../components/StepControl";
import Cart from "./../components/Cart";

// 建立購物車清單資料庫，未來若越來越多品項，用v-for跑迴圈效率高
const dummyData = {
  carts: [
    {
      id: 0,
      name: "刷色直筒牛仔褲",
      image: "https://upload.cc/i1/2022/08/22/wMgbhI.png",
      price: 1299,
      quantity: 1,
    },
    {
      id: 1,
      name: "破壞補丁修身牛仔褲",
      image: "https://upload.cc/i1/2022/08/24/VNFL8d.png",
      price: 3999,
      quantity: 1,
    },
  ],
  total: 0,
  steps: [1, 2, 3, 4],
  currentStep: 1,
  deliveryFee: false,
  form: {
    salutation: "",
    username: "",
    phone: "",
    email: "",
    city: "",
    addr: "",
    deliveryFee: 0,
    ccname: "",
    cardnumber: "",
    expdate: "",
    cvv: "",
    totalPrice: 0,
  },
};

export default {
  name: "Main",
  components: {
    Stepper,
    Form,
    StepControl,
    Cart,
  },
  data() {
    return {
      carts: [],
      total: 0,
      steps: [],
      currentStep: 0,
      deliveryFee: false,
      form: {},
    };
  },
  created() {
    this.fetchData();
  },
  // 因為total的資料是在父層，所以watch監聽放在父層
  watch: {
    // 讓電腦自己去監聽carts裡的資料改變，有改變數量就重新渲染總額
    carts: {
      handler: function () {
        this.calculateTotal();
      },
      deep: true,
    },
    deliveryFee: {
      handler: function () {
        this.calculateTotal();
      },
      deep: true,
    },
  },
  methods: {
    fetchData() {
      const { carts, total, steps, currentStep, deliveryFee, form } = dummyData;
      this.carts = carts;
      this.total = total;
      this.steps = steps;
      this.currentStep = currentStep;
      this.deliveryFee = deliveryFee;
      this.form = form;
    },
    // 若點擊確認訂單，會跳成第一步；若點擊取消，回到第三步
    clickModalPageButton(e){
      return e.target.matches('.btn-primary') ? this.currentStep = 1 : this.currentStep = 3
    },
    // 實作增加按鈕，不需要再使用forEach
    afterAddQuantity(cartId) {
      this.carts[cartId].quantity++;
    },
    // 實作減少按鈕
    afterDeleteQuantity(cartId) {
      this.carts[cartId].quantity && this.carts[cartId].quantity--;
    },
    // 實作下一步，最後一頁不可以再按下一步
    afterAddStep() {
      if (this.currentStep < this.steps.length - 1) {
        this.currentStep++;
      } else {
        if(Object.values(this.form).every(value => String(value).trim().length>0)){
          this.currentStep++;
        }else {
          this.currentStep
          alert('有資料尚未填寫完成。')
        }
      }
    },
    // 實作上一步，第一頁不可以再按上一步
    afterMinusStep() {
      if (this.currentStep > 0) {
        this.currentStep--;
      }
    },
    // 計算運費為500
    afterAddDeliveryFee() {
      this.deliveryFee = 500;
    },
    // 計算運費為0
    afterNoDeliveryFee() {
      this.deliveryFee = 0;
    },
    afterFormChange(form) {
      return (this.form = form);
    },
    afterFinishForm() {
      return (this.form = {
        ...this.form,
        total: this.total,
      });
    },
    // 將運費加進總額裡面，因為預設值為false，所以一開始若點擊標準運送就不影響total
    calculateTotal() {
      let money = 0;
      this.carts.forEach((cart) => {
        money = money + cart.price * cart.quantity;
      });
      return (this.total = money + this.deliveryFee);
    },
  },
};
</script>


<!-- 載入SCSS -->
<style lang="scss">
@import "../assets/main.scss";

hr {
  border: 0px;
  height: 1px;
  background: var(--cart);
}

.seperate-line {
  margin-top: 2rem;
}

.container {
  // 讓整體區塊置中
  width: 80%;
  position: relative;
  margin: 0 auto;
}

#modalBackground {
  position: fixed;
  width: 100vw;
  height: 100%;
  opacity: 0.7;
  background-color: black;
  z-index: 998;
}

#modalPage {
  border-radius: 0.5rem;
  width: 30vw;
  height: 30vw;
  background-color: var(--white);
  position: absolute;
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%);
  z-index: 999;
}

#modalPageContainer {
  line-height: 0.5rem;
  font-size: 0.5rem;
  width: 80%;
  height: 80%;
  position: absolute;
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%);
  z-index: 999;
}

// main
main {
  width: 100%;
  padding: 1rem 0;
  h1 {
    font-size: 1.5rem;
    margin: 1rem 0;
  }
  .main-content {
    width: 100%;
    display: flex;
    justify-content: space-between;
    .left-content {
      flex: 50% 0 0;
    }

    .right-content {
      margin-top: 2rem;
      flex: 40% 0 0;
      width: 100%;
    }
  }
}
</style>