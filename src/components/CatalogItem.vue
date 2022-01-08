<template>
  <div class="catalog__item">
    <div class="item__header">
      <div class="item__image">
        <img :src="product.image" alt="" width="240" height="240" />
      </div>
      <button v-if="!isCart" @click="handleFav" class="fav-btn">
        <icon-heart :class="{ active: product.isFav }" />
      </button>
    </div>
    <div class="item__body">
      <div class="item__price">{{ product.price }} ₽</div>
      <p class="item__name">{{ product.dish }}</p>
      <div class="item__buttons">
        <button
          v-if="!isCart && !isInCart"
          class="item__button"
          @click="addToCart"
        >
          В корзину
        </button>
        <router-link
          v-else-if="!isCart && isInCart"
          class="item__button picked"
          to="Cart"
          >К корзине
        </router-link>
        <div v-if="isInCart || isCart" class="item__quantity">
          <button @click="decreaseProductQuantity">-</button>
          <input v-model="product.quantInCart" readonly />
          <button @click="increaseProductQuantity">+</button>
        </div>
      </div>

      <div v-if="isCart" class="item__options">
        <a @click.prevent="handleFav">
          <span v-if="!product.isFav"> В избранное </span>
          <span v-else class="red">В избранном</span>
        </a>
        <a @click.prevent="removeFromCart">Удалить</a>
      </div>
    </div>
  </div>
</template>

<script>
import IconHeart from "./icons/IconHeart.vue";
export default {
  components: {
    IconHeart,
  },
  props: {
    product: {
      type: Object,
      required: true,
    },
  },
  methods: {
    addToCart() {
      this.$store.commit("addToCart", this.product);
    },
    handleFav() {
      if (this.product.isFav) {
        this.$store.commit("removeFromFav", this.product);
      } else {
        this.$store.commit("addToFav", this.product);
      }
    },
    removeFromCart() {
      this.$store.commit("removeFromCart", this.product);
    },
    increaseProductQuantity() {
      this.product.quantInCart++;
    },
    decreaseProductQuantity() {
      if (this.product.quantInCart === 1) {
        this.removeFromCart();
      } else {
        this.product.quantInCart--;
      }
    },
  },
  computed: {
    isCart() {
      return this.$route.name === "Cart";
    },
    isInCart() {
      return this.product.quantInCart > 0;
    },
  },
};
</script>

<style lang="scss" scoped>
.catalog__item {
  display: flex;
  flex-direction: column;
  &.cart__item {
    flex-direction: row;
    align-items: center;
    gap: 20px;
  }
  .item {
    &__header {
      position: relative;
      .fav-btn {
        position: absolute;
        top: 10px;
        right: 10px;
        border-radius: 50%;
        border: none;
        width: 30px;
        height: 30px;
        display: flex;
        justify-content: center;
        align-items: center;
        cursor: pointer;
        svg {
          width: 20px;
        }
      }
    }
    &__image {
      width: 100%;
      max-width: 240px;
      img {
        display: block;
        width: 100%;
        height: auto;
      }
    }
    &__body {
      display: flex;
      flex-direction: column;
      padding: 10px 0;
      flex: 1;
    }
    &__price {
      font-weight: 700;
      font-size: 20px;
      margin-bottom: 10px;
    }
    &__name {
      margin-bottom: 15px;
    }
    &__buttons {
      display: flex;
      justify-content: space-between;
    }
    &__button {
      width: 100%;
      margin-top: auto;
      padding: 10px 20px;
      background: #fdd51f;
      color: #1f1f1d;
      text-align: center;
      border-radius: 6px;
      border: none;
      cursor: pointer;
      transition: 0.2s ease;
      text-decoration: none;
      &:hover {
        background: #e6c017;
      }
      &.picked {
        background: #741ffd;
        color: #fff;
        width: auto;
        &:hover {
          background: #6315e0;
        }
      }
    }
    &__quantity {
      display: flex;

      input {
        max-width: 30px;
        text-align: center;
        font-size: 18px;
        border: 2px solid #eff3f6;
      }
      button {
        padding: 5px;
        width: 30px;
        font-size: 20px;
        line-height: 1;
        background: #eff3f6;
        color: #fdd51f;
        border: 2px solid #eff3f6;
        border-radius: 6px 0 0 6px;
        cursor: pointer;
        &:last-of-type {
          border-radius: 0 6px 6px 0;
        }
      }
    }
    &__options {
      margin: 10px 0;
      display: flex;
      gap: 10px;
      a {
        color: #fdd51f;
        cursor: pointer;
        .red {
          color: #c92471;
        }
      }
    }
  }
}
</style>
