<template>
    <div class="title">
        <div class="line"></div>
        <div class="text">{{ltext}}</div>
        <div class="line"></div>
    </div>
</template>

<script>
export default {
  props: ["ltext"]
};
</script>

<style lang="stylus" rel="stylesheet/stylus">
@import '../../common/stylus/mixin.styl';

.title {
    display: flex;
    width: 80%;
    margin: 28px auto 24px auto;

    .line {
        flex: 1;
        position: relative;
        top: -6px;
        border-bottom: 1px solid rgba(255, 255, 255, 0.2);
    }

    .text {
        padding: 0px 12px;
        font-weight: 700;
        font-size: 14px;
    }
}
</style>
