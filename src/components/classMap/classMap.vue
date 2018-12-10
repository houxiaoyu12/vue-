<template>
    <div class="head">
        <span class="icon1" :class="iconClass" v-if="isicon===1"></span>
        <span class="icon2" :class="iconClass" v-if="isicon===2"></span>
    </div>
</template>

<script>
export default {
  props: ["iconClass", "isicon"],
  created() {
    this.classMap = ["decrease", "discount", "special", "invoice", "guarantee"];
  }
};
</script>

<style lang="stylus" rel="stylesheet/stylus">
@import '../../common/stylus/mixin.styl';

.head {
    display: inline;
}

.icon1 {
    vertical-align: top;
    display: inline-block;
    width: 12px;
    height: 12px;
    margin-right: 4px;
    background-size: 12px 12px;
    background-repeat: none;

    &.decrease {
        bg-image('decrease_1');
    }

    &.discount {
        bg-image('discount_1');
    }

    &.invoice {
        bg-image('invoice_1');
    }

    &.special {
        bg-image('special_1');
    }

    &.guarantee {
        bg-image('guarantee_1');
    }
}

.icon2 {
    display: inline-block;
    width: 16px;
    height: 16px;
    vertical-align: top;
    margin-right: 6px;
    background-size: 16px 16px;
    background-repeat: no-repeat;

    &.decrease {
        bg-image('decrease_2');
    }

    &.discount {
        bg-image('discount_2');
    }

    &.invoice {
        bg-image('invoice_2');
    }

    &.special {
        bg-image('special_2');
    }

    &.guarantee {
        bg-image('guarantee_2');
    }
}
</style>
