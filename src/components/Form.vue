<template>
  <div class="form-container">
    <form action="" id="a-form">
      <div class="form-content">
        <!--       輸入框１       -->
        <div 
        class="form-content__part"
        v-show="currentStep === 1">
          <h4>寄送地址</h4>
          <div class="form-row-container-1">
            <div class="form-row form-row-title">
              <label>稱謂</label>
              <div class="select-wrapper">
                <select 
                  name="a-type" 
                  id="a-type" 
                  required 
                  v-model="form.salutation">
                  <option value="" disabled selected>請選擇</option>
                  <option value="Mister">先生</option>
                  <option value="Miss">小姐</option>
                </select>
              </div>
            </div>
            <div class="form-row form-row-name">
              <label for="">姓名</label>
              <input 
                id="name" 
                type="text" 
                placeholder="請輸入姓名"
                v-model="form.name" />
            </div>
            <div class="form-row form-row-mobile">
              <label for="">電話</label>
              <input 
                id="mobile" 
                type="text" 
                placeholder="請輸入行動電話"
                v-model="form.phone" />
            </div>
            <div class="form-row form-row-email">
              <label for="">Email</label>
              <input 
                id="email" 
                type="text" 
                placeholder="請輸入電子郵件"
                v-model="form.email" />
            </div>
            <div class="form-row form-row-county">
              <label>縣市</label>
              <div class="select-wrapper">
                <select 
                name="a-type" 
                id="a-type" 
                required 
                v-model="form.city">
                  <option value="" disabled selected>請選擇</option>
                  <option value="Kaohsiung">高雄市</option>
                  <option value="Taipei">台北市</option>
                  <option value="New Taipei">新北市</option>
                </select>
              </div>
            </div>
            <div class="form-row form-row-address">
              <label for="">地址</label>
              <input 
              id="address" 
              type="text" 
              placeholder="請輸入地址"
              v-model="form.addr" />
            </div>
          </div>
        </div>
        <!--       輸入框２       -->
        <div 
        class="form-content__part"
        v-show="currentStep === 2">
          <h4>運送方式</h4>
          <div class="form-row-container-2">
            <form class="form-card-container">
              <div class="form-card">
                <input
                  type="radio"
                  id="form-card-1"
                  name="form"
                  class="form-circle"
                  value="0"
                  @change="noDeliveryFee"
                  v-model="form.deliveryFee"
                />
                <label for="form-card-1" class="form-text">
                  <div class="form-text__up">
                    <p>標準運送</p>
                    <p>免費</p>
                  </div>
                  <div class="form-text__down">
                    <p>約3~7個工作天</p>
                  </div>
                </label>
              </div>
              <div class="form-card">
                <input
                  type="radio"
                  id="form-card-2"
                  name="form"
                  class="form-circle"
                  value="500"
                  @change="addDeliveryFee"
                  v-model="form.deliveryFee"
                />
                <label for="form-card-2" class="form-text">
                  <div class="form-text__up">
                    <p>DHL 貨運</p>
                    <p>$500</p>
                  </div>
                  <div class="form-text__down">
                    <p>48小時內送達</p>
                  </div>
                </label>
              </div>
            </form>
          </div>
        </div>
        <!--       輸入框３       -->
        <div 
        class="form-content__part"
        v-show="currentStep >= 3">
          <h4>付款資訊</h4>
          <div class="form-row-container-3">
            <div class="form-row form-row-bank-name">
              <label for="">持卡人姓名</label>
              <input 
              id="bank-name" 
              type="text" 
              placeholder="John Doe"
              v-model="form.ccname" />
            </div>
            <div class="form-row form-row-bank-code">
              <label for="">卡號</label>
              <input
                id="bank-code"
                type="text"
                placeholder="1111 2222 3333 4444"
                class="w-50"
                v-model="form.cardnumber"
              />
            </div>
            <div class="form-row form-row-bank-date">
              <label for="">有效日期</label>
              <input 
              id="account-date" 
              type="text" 
              placeholder="MM/YY" 
              v-model="form.expdate"/>
            </div>
            <div class="form-row form-row-bank-CVC">
              <label for="">CVC/CCV</label>
              <input 
              id="account-CVC" 
              type="text" 
              placeholder="123" 
              v-model="form.cvv"/>
            </div>
          </div>
        </div>
      </div>
    </form>
  </div>
</template>

<script>
export default {
  name: 'Form',
  props:{
    currentStep:{
      type: Number,
      default: 1 // 預設為１
    },
    initialForm:{
      type: Object,
      default: () => {
        return {
          salutation: '',
          username: '',
          phone: '',
          email: '',
          city: '',
          addr: '',
          deliveryFee: 0,
          ccname: '',
          cardnumber: '',
          expdate: '',
          cvv: '',
          totalPrice: 0
        }
      }
    },
    total:{
      type: Number,
      required: true
    }
  },
  watch: {
    form: {
      handler: function() {
        this.formChange()
      },
      deep: true
    }
  },
  data(){
    return {
      form : this.initialForm
    }
  },
  methods: {
    addDeliveryFee(){
      this.$emit('after-add-delivery-fee')
    },
    noDeliveryFee(){
      this.$emit('after-no-delivery-fee')
    },
    formChange(){
      this.$emit('after-form-change',this.form)
    }
  }

}
</script>


<style scoped lang="scss">
@import "../assets/main.scss";
.form-container {
  width: 100%;
  .form-content__part {
    width: 100%;
    h4 {
      font-size: 1.5rem;
    }
    // 寄送地址
    .form-row-container-1 {
      @extend %form-row-container; // 繼承與複寫樣式
      grid-template-columns: repeat(6, auto);
      grid-column-gap: 1.5rem;
      grid-template-areas:  // grid可以快速切好二維排版
      "title title name name name name"
      "mobile mobile mobile email email email"
      "county county address address address address";

      .form-row-title {
        grid-area: title;
      }
      .form-row-name {
        grid-area: name;
      }
      .form-row-mobile {
        grid-area: mobile;
      }
      .form-row-email {
        grid-area: email;
      }
      .form-row-county {
        grid-area: county;
      }
      .form-row-address {
        grid-area: address;
      }
    }
    // 運送方式
    .form-row-container-2 {
      display: block;
      width: 100%;
      .form-card {
        margin-top: 1.5rem;
        width: 100%;
        border: 1px solid var(--cart);
        border-radius: 0.3rem;
        display: flex;
        .form-circle {
          appearance: none; // 清空樣式
          text-align: center;
          margin: 1.25rem;
          height: 1.25rem;
          width: 1.25rem;
          border: 0.1rem solid var(--number);
          border-radius: 50%;
        }
        .form-circle:checked {
          text-align: center;
          margin: 1.25rem;
          height: 1.25rem;
          width: 1.25rem;
          border: 0.375rem solid var(--number);
          border-radius: 50%;
        }
        .form-text {
          margin: auto 0;
          width: 100%;
          &__up {
            width: 100%;
            padding-right: 1.25rem;
            display: flex;
            justify-content: space-between;
          }
        }
      }
      .checked {
        border: 1px solid var(--number);
      }
    }
    // 付款資訊
    .form-row-container-3 {
      @extend %form-row-container; // 繼承與複寫樣式
      grid-template-columns: repeat(6, auto);
      grid-column-gap: 1.5rem;
      grid-template-areas: // grid可以快速切好二維排版
        "bank-name bank-name bank-name bank-name . ."
        "bank-code bank-code bank-code bank-code . ."
        "bank-date bank-date bank-date bank-CVC bank-CVC bank-CVC ";
      .form-row-bank-name {
        grid-area: bank-name;
      }
      .form-row-bank-code {
        grid-area: bank-code;
      }
      .form-row-bank-date {
        grid-area: bank-date;
      }
      .form-row-bank-CVC {
        grid-area: bank-CVC;
      }
    }
  }
}
</style>