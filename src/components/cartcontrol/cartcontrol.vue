<template>
  <div class="cartcontrol">
    <transition name="move">
      <div class="cart-decrease" v-show="food.count>0" @click="decreaseCart()">
        <span class="inner icon-remove_circle_outline"></span>
      </div>
    </transition>
    <div class="cart-count" v-show="food.count>0">{{food.count}}</div>
    <div class="cart-add icon-add_circle" @click="addCart"></div>
  </div>
</template>
<script>
import Vue from "vue";
export default {
  props: {
    food: {
      type: Object
    }
  },
  methods: {
    addCart(event) {
      if (!this.food.count) {
        Vue.set(this.food, "count", 1);
      } else {
        this.food.count++;
      }
      this.$emit("cartadd", event.target);
    },
    decreaseCart() {
      if (this.food.count > 0) {
        this.food.count--;
      }
    }
  }
};
</script>

<style lang="stylus" rel="stylesheet/stylus">
.cartcontrol {
  font-size: 0;

  .cart-decrease {
    display: inline-block;
    padding: 6px;
    transition: all 0.4s linear;
    font-size: 10px;

    &.move-transition {
      opacity: 1;
      transform: translate3d(0, 0, 0);
    }

    .inner {
      line-height: 24px;
      font-size: 24px;
      color: rgb(0, 160, 220);
    }

    &.move-enter, &.move-leave {
      opacity: 0;
      transform: translate3d(24px, 0, 0);
    }
  }

  .cart-count {
    display: inline-block;
    font-size: 10px;
    vertical-align: top;
    width: 12px;
    padding-top: 6px;
    line-height: 24px;
    text-align: center;
    color: rgb(147, 153, 159);
  }

  .cart-add {
    display: inline-block;
    padding: 6px;
    line-height: 24px;
    font-size: 24px;
    color: rgb(0, 160, 220);
  }
}
</style>
