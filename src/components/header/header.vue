<template>
    <div class="header">
        <!-- 顶部区域 -->
        <div class="content-wrapper">
            <div class="avatar">
                <img :src="seller.avatar" width="64" height="64">
            </div>
            <div class="content">
                <div class="title">
                    <span class="brand"></span>
                    <span class="name">{{seller.name}}</span>
                </div>
                <div class="description">
                    {{seller.description}}/{{seller.deliveryTime}}分钟送达
                </div>
                <div v-if="seller.supports" class="support">
                    <mapclass :iconClass="classMap[seller.supports[0].type]" :isicon="1"></mapclass>
                    <span class="text">{{seller.supports[0].description}}</span>
                </div>
            </div>
            <div v-if="seller.supports" class="support-count" @click="showdetail()">
                <span class="count">{{seller.supports.length}}个</span>
                <i class="icon-keyboard_arrow_right"></i>
            </div>
        </div>
        <!-- 公告区域 -->
        <div class="bulletin-wrapper" @click="showdetail()">
            <span class="bulletin-title"></span>
            <span class="bulletin-text">{{seller.bulletin}}</span>
             <i class="icon-keyboard_arrow_right"></i>
        </div>
        <!-- 公告区域背景 -->
        <div class="background">
            <img :src="seller.avatar" width="100%" height="100%">
        </div>

        <!-- 弹出框 -->
        <div class="detail" v-show="detailShow">
            <div class="detail-wrapper clearfix">
                <div class="detail-main">
                  <h1 class="name">{{seller.name}}</h1>
                  <div class="star-wrapper">
                        <star :score="seller.score" :size="48"></star>
                  </div> 
                      <lineText :ltext="'优惠信息'"></lineText>
                  <ul v-if="seller.supports" class="supports">
                      <li class="support-item" v-for="(item,key) in seller.supports" :key="key">
                          <mapclass :iconClass="classMap[seller.supports[key].type]" :isicon="2"></mapclass>
                          <span class="text">{{seller.supports[key].description}}</span>
                      </li>
                  </ul>
                  <lineText :ltext="'商家公告'"></lineText>

                  <div class="bulletin">
                      <p class="content">{{seller.bulletin}}</p>
                  </div>  
                </div>
            </div>
            <!-- 弹出框关闭按钮 -->
            <div class="detail-close" @click="closeDetail()">
                <i class="icon-close"></i>
            </div>
        </div>
    </div>
</template>

<script>
import star from "./../star/star";
import lineText from "./../lineText/lineText";
import mapclass from "./../classMap/classMap";
export default {
  props: {
    seller: {
      type: Object
    }
  },
  created() {
    this.classMap = ["decrease", "discount", "special", "invoice", "guarantee"];
  },
  data() {
    return {
      detailShow: false
    };
  },
  methods: {
    showdetail() {
      this.detailShow = true;
    },
    closeDetail() {
      this.detailShow = false;
    }
  },
  components: {
    star: star,
    lineText: lineText,
    mapclass: mapclass
  }
};
</script>

<style lang="stylus" rel="stylesheet/stylus">
@import '../../common/stylus/mixin.styl';

.header {
    color: #fff;
    position: relative;
    background: rgba(7, 17, 27, 0.4);
    overflow: hidden;

    .content-wrapper {
        padding: 24px 12px 18px 24px;
        font-size: 0;
        position: relative;

        .avatar {
            display: inline-block;
            font-size: 14px;
            vertical-align: top;

            img {
                border-radius: 2px;
            }
        }

        .content {
            display: inline-block;
            margin-left: 15px;

            .title {
                margin: 2px 0 8px 0;

                .brand {
                    width: 38px;
                    height: 18px;
                    display: inline-block;
                    bg-image('brand');
                    background-size: 30px 18px;
                    background-repeat: none;
                    vertical-align: top;
                }

                .name {
                    margin-left: 6px;
                    font-size: 16px;
                    line-height: 18px;
                    font-weight: bold;
                    width: 200px;
                }
            }

            .description {
                margin-bottom: 6px;
                line-height: 18px;
                font-size: 12px;
            }

            .support {
                .text {
                    line-height: 12px;
                    font-size: 10px;
                }
            }
        }

        .support-count {
            position: absolute;
            right: 12px;
            bottom: 14px;
            padding: 0 8px;
            height: 24px;
            line-height: 24px;
            border-radius: 14px;
            background: rgba(0, 0, 0, 0.2);
            text-align: center;

            .count {
                font-size: 10px;
            }

            .icon-keyboard_arrow_right {
                font-size: 10px;
            }
        }
    }

    .bulletin-wrapper {
        height: 28px;
        line-height: 28px;
        padding: 0px 22px 0 12px;
        white-space: nowrap;
        overflow: hidden;
        text-overflow: ellipsis;
        position: relative;
        background: rgba(7, 17, 27, 0.2);

        .bulletin-title {
            display: inline-block;
            width: 22px;
            height: 12px;
            bg-image('bulletin');
            background-size: 22px 12px;
            background-repeat: no-repeats;
            vertical-align: top;
            margin-top: 8px;
        }

        .bulletin-text {
            font-size: 10px;
            margin: 0 4px;
            vertical-align: top;
        }

        .icon-keyboard_arrow_right {
            position: absolute;
            font-size: 10px;
            right: 12px;
            top: 8px;
        }
    }

    .background {
        position: absolute;
        top: 0px;
        left: 0px;
        width: 100%;
        height: 100%;
        z-index: -1;
        filter: blur(10px);
    }

    .detail {
        position: fixed;
        z-index: 100;
        width: 100%;
        height: 100%;
        left: 0px;
        top: 0px;
        overflow: auto;
        background: rgba(7, 17, 27, 0.8);

        .detail-wrapper {
            min-height: 100%;
            width: 100%;

            .detail-main {
                margin-top: 64px;
                padding-bottom: 64px;

                .name {
                    line-height: 16px;
                    text-align: center;
                    font-size: 16px;
                    font-weight: 700;
                }

                .star-wrapper {
                    margin-top: 18px;
                    padding: 2px 0;
                    text-align: center;
                }

                .supports {
                    width: 80%;
                    margin: 0 auto;

                    .support-item {
                        padding: 0 12px;
                        margin-bottom: 12px;
                        font-size: 0;

                        &:last-child {
                            margin-bottom: 0;
                        }

                        .text {
                            font-size: 12px;
                            line-height: 16px;
                        }
                    }
                }

                .bulletin {
                    width: 80%;
                    margin: 0 auto;

                    .content {
                        padding: 0 12px;
                        line-height: 24px;
                        font-size: 12px;
                    }
                }
            }
        }

        .detail-close {
            position: relative;
            width: 32px;
            height: 32px;
            margin: -64px auto 0 auto;
            clear: both;
            font-size: 32px;
        }
    }
}
</style>
