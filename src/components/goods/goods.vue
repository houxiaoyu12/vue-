<template>
  <div class="goods">
    <div class="menu-wrapper" ref="menuwrapper">
      <ul>
        <li
          v-for="(item,key) in goods"
          :key="key"
          class="menu-item"
          :class="{'current':currentIndex===key}"
          @click="selectMenu(key,$event)"
        >
          <span class="text border-1px">
            <mapclass :iconClass="classMap[item.type]" :isicon="1" v-show="item.type>0"></mapclass>
            {{item.name}}
          </span>
        </li>
      </ul>
    </div>
    <div class="foods-wrapper" ref="foodsWrapper">
      <ul>
        <li class="food-list food-list-hook" v-for="(item,key) in goods" :key="key">
          <h1 class="title">{{item.name}}</h1>
          <ul>
            <li class="food-item border-1px" v-for="(food,key) in item.foods" :key="key">
              <div class="icon">
                <img width="57" :src="food.icon">
              </div>
              <div class="content">
                <h2 class="name">{{food.name}}</h2>
                <p class="desc">{{food.description}}</p>
                <div class="extra">
                  <span class="count">月售{{food.sellCount}}份</span>
                  <span>好评率:{{food.rating}}%</span>
                  <div class="price">
                    <span class="now">￥{{food.price}}</span>
                    <span class="old" v-show="food.oldPrice">￥{{food.price}}</span>
                  </div>
                  <div class="cartcontrol-wrapper">
                    <cartcontrol :food="food" @cartadd="cartadd"></cartcontrol>
                  </div>
                </div>
              </div>
            </li>
          </ul>
        </li>
      </ul>
    </div>
    <shopcar
      :deliveryPrice="seller.deliveryPrice"
      :minPrice="seller.inPrice"
      :select-foods="selectFoods"
      ref="shopcar"
    ></shopcar>
  </div>
</template>

<script>
import mapclass from "./../classMap/classMap";
import BScroll from "better-scroll";
import shopcar from "./../shopcar/shopcar";
import cartcontrol from "./../cartcontrol/cartcontrol";
const ERR_OK = 0;
export default {
  props: ["seller"],
  data() {
    return {
      goods: [],
      listHeight: [],
      scrollY: 0
    };
  },
  created() {
    this.$http.get("/api/goods").then(res => {
      this.goods = res.data.data;
      //  $nextTick :在下次 DOM 更新循环结束之后执行延迟回调。在修改数据之后立即使用这个方法，获取更新后的 DOM
      this.$nextTick(() => {
        this.initScroll();
        this._calculateHeight();
      });
    });
    this.classMap = ["decrease", "discount", "special", "invoice", "guarantee"];
  },
  methods: {
    initScroll() {
      this.menuScroll = new BScroll(this.$refs.menuwrapper, {
        click: true
      });
      this.foodsScroll = new BScroll(this.$refs.foodsWrapper, {
        probeType: 3,
        click: true
      });
      this.foodsScroll.on("scroll", pos => {
        this.scrollY = Math.abs(Math.round(pos.y));
      });
    },
    _calculateHeight() {
      let foodlist = this.$refs.foodsWrapper.getElementsByClassName(
        "food-list-hook"
      );
      let height = 0;
      this.listHeight.push(height);
      for (let index = 0; index < foodlist.length; index++) {
        const element = foodlist[index];
        height += element.clientHeight;
        this.listHeight.push(height);
      }
    },
    selectMenu(key, $event) {
      let foodlist = this.$refs.foodsWrapper.getElementsByClassName(
        "food-list-hook"
      );
      let el = foodlist[key];
      this.foodsScroll.scrollToElement(el, 300);
    },
    cartadd(el) {
      //优化体验，异步执行下落动画
      this.$nextTick(() => {
        this.$refs.shopcar.drop(el);
      });
    }
  },
  components: {
    mapclass: mapclass,
    shopcar: shopcar,
    cartcontrol: cartcontrol
  },
  computed: {
    currentIndex() {
      for (let index = 0; index < this.listHeight.length; index++) {
        let height1 = this.listHeight[index];
        let height2 = this.listHeight[index + 1];
        if (!height2 || (this.scrollY >= height1 && this.scrollY < height2)) {
          return index;
        }
      }
      return 0;
    },
    selectFoods() {
      let foods = [];
      this.goods.forEach(good => {
        good.foods.forEach(food => {
          if (food.count) {
            foods.push(food);
          }
        });
      });
      return foods;
    }
  }
};
</script>

<style lang="stylus" rel="stylesheet/stylus">
@import '../../common/stylus/mixin.styl';

.goods {
  display: flex;
  position: absolute;
  top: 174px;
  bottom: 46px;
  width: 100%;
  overflow: hidden;

  .menu-wrapper {
    flex: 0 0 80px;
    width: 80px;
    background: #f3f5f7;

    .menu-item {
      display: table;
      height: 54px;
      width: 56px;
      padding: 0 12px;
      line-height: 14px;

      &.current {
        font-weight: bold;
        background-color: #fff;
        position: relative;
        z-index: 10;
        margin-top: -1px;

        &.text {
          border-none();
        }
      }

      .icon {
        margin-right: 2px;
      }

      .text {
        display: table-cell;
        width: 56px;
        vertical-align: middle;
        border-1px(rgba(7, 17, 27, 0.1));
        font-size: 12px;
      }
    }
  }

  .foods-wrapper {
    flex: 1;

    .title {
      padding-left: 14px;
      height: 26px;
      line-height: 26px;
      border-left: 2px solid #d9dde1;
      font-size: 12px;
      color: rgb(147, 153, 159);
      background: #f3f5f7;
    }

    .food-item {
      display: flex;
      margin: 18px;
      padding-bottom: 18px;
      border-1px(rgba(7, 17, 27, 0.1));

      &:last-child {
        border-none();
        margin-bottom: 0;
      }

      .icon {
        flex: 0 0 57px;
        margin-right: 10px;
      }

      .content {
        flex: 1;

        .name {
          margin: 2px 0 8px 0;
          height: 14px;
          line-height: 14px;
          font-size: 14px;
          color: rgb(7, 17, 27);
        }

        .desc {
          margin-bottom: 8px;
          line-height: 10px;
          font-size: 10px;
          color: rgb(147, 153, 159);
        }

        .extra {
          line-height: 10px;
          font-size: 10px;
          color: rgb(147, 153, 159);

          .count {
            margin-right: 12px;
          }
        }

        .price {
          font-weight: 700;
          line-height: 24px;

          .now {
            margin-right: 8px;
            font-size: 14px;
            color: rgb(240, 20, 20);
          }

          .old {
            text-decoration: line-through;
            font-size: 10px;
            color: rgb(147, 153, 159);
          }
        }

        .cartcontrol-wrapper {
          position: absolute;
          right: 0;
          bottom: 12px;
        }
      }
    }
  }
}
</style>
