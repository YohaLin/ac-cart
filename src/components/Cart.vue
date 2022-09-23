<template>
  <div class="cart-container">
    <h5>購物籃</h5>
    <div class="cart-cards">
      <div class="cart-card" v-for="cart in carts" :key="cart.id">
        <!-- 給予id -->
        <div class="cart-card__image">
          <img :src="cart.image" alt="grocery" />
        </div>
        <div class="cart-card__content">
          <div class="cart-card__content-title">
            <p>{{cart.name}}</p>
            <div class="cart-card__content-title--quantity">
              <!-- 加減按鈕上綁定v-on事件 -->
              <div 
                class="cart-card-circle-container btn-minus"
                @click.stop.prevent="deleteQuantity(cart.id)"
              >
              -
              </div>
              <div class="cart-card-number">{{cart.quantity}}</div>
              <div 
                class="cart-card-circle-container btn-add"
                @click.stop.prevent="addQuantity(cart.id)"
              >
              +
              </div>
            </div>
          </div>
          <p class="cart-card__content-money">${{cart.price}}</p>
        </div>
      </div>
    </div>
    <!--       價錢       -->
    <div class="cart-money">
      <hr />
      <div class="cart-money__container">
        <div class="cart-money__container-title">運費</div>
        <div class="cart-money__container-money">免費</div>
      </div>
      <hr />
      <div class="cart-money__container">
        <div class="cart-money__container-title">小計</div>
        <div class="cart-money__container-money" id="total">${{total}}</div>
      </div>
    </div>
  </div>
</template>

<script>
/*eslint-disable*/
export default {
  name: 'Cart',
  props: {
    initialCarts: {
      type: Array,
      required: true
    },
    total: {
      type: Number,
      default: 5298
    }
  },
  data() {
    return {
      carts: this.initialCarts
    }
  },
  methods: {
    // 實作增加按鈕
    addQuantity(cartId){
      this.$emit('after-add-quantity', cartId)
    },
    // 實作減少按鈕
    deleteQuantity(cartId){
      this.$emit('after-delete-quantity', cartId)
    },
  },
}
</script>

<style lang="scss">
@import "../assets/main.scss";

.cart-container {
  background: var(--cart-bg);
  border: 1px solid var(--cart);
  border-radius: 0.5rem;
  h5 {
    display: block;
    font-size: 1.5rem;
    margin: 2rem 0 1.5rem 1.5rem;
  }
  .cart-cards {
    width: 90%;
    margin: 0 auto;
    .cart-card {
      margin-top: 1rem;
      display: flex;
      justify-content: space-between;
      &__image img {
        width: 6rem;
        height: 6rem;
      }
      &__content {
        width: 100%;
        display: flex;
        justify-content: space-between;
        // align-items: flex-start;
        margin-left: 2rem;
        flex: 1;
        &-title {
          &--quantity {
            margin-top: 1rem;
            display: flex;
            justify-content: start;
            div:nth-child(1),
            div:nth-child(3) {
              color: var(--add-minus);
              height: 1.5rem;
              width: 1.5rem;
              text-align: center;
              background-color: var(--cart);
              border-radius: 50%;
              cursor: pointer;
            }
            div:nth-child(2) {
              margin: 0 1rem;
            }
          }
        }
      }
        &-money {
          margin-top: 0;
        }
    }
  }
  .cart-money {
    margin: 1rem auto 0 auto;
    width: 90%;
    &__container {
      display: flex;
      justify-content: space-between;
      margin: 1rem 0;
    }
  }
}
</style>