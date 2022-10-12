<template>
  <div class="dynamic-details">
    <div class="dynamic-details-header">
      <div class="dynamic-details-header-arrow" @click="JumpRouter">
        <span class="iconfont icon-arrow-left"></span>
      </div>
      <div class="dynamic-details-header-text">详情</div>
    </div>
    <div class="dynamic-details-body">
      <div class="dynamic-body-content">
        <div class="dynamic-body-header">
          <div class="body-header-logo">
            <img :src="item.headimage ? secondUrl[index] : secondUrl[7]" />
          </div>
          <div class="body-header-items">
            <div class="body-header-text">{{ item.username }}</div>
            <div class="body-header-times">
              <span>{{ item.timeline }}</span>
            </div>
          </div>
        </div>
        <div class="dynamic-body-image" @click="showPicture(index)">
          <img
            :src="mainSrc[index] ? mainSrc[index] : null"
            v-if="mainSrc[index]"
          />
        </div>
        <!-- 预览照片 -->
        <div class="body-header-img-lay" v-if="isShowLay">
          <div class="body-lay-header" v-show="!isShowHeader">
            <div class="lay-header-left">
              <div class="lay-header-arrow" @click="showElement">
                <span class="iconfont icon-zuojiantou"></span>
              </div>
              <div class="lay-header-text">图片</div>
            </div>
            <div class="lay-header-right">
              <a
                :href="mainSrc[showCt] ? mainSrc[showCt] : null"
                download="pircture.jpg"
              >
                <span class="iconfont icon-download"></span
              ></a>
            </div>
          </div>
          <div class="body-lay-img" @click="isShowHeader = !isShowHeader">
            <div class="lay-ig">
              <img :src="mainSrc[showCt] ? mainSrc[showCt] : null" />
            </div>
          </div>
        </div>
        <!-- 预览结束 -->
        <div class="dynamic-body-words">{{ item.content }}</div>
        <div class="dynamic-body-footer">
          <div class="body-footer-thumbs">
            <img src="../../assets/image/user-icon/like.png" ref="touch" />
            <div class="touch-btn" @click="praiseAdd">
              {{ item.pmax }}
            </div>
          </div>
          <div class="body-footer-comment" @click="show = !show">
            <img src="../../assets/image/user-icon/comment.png" />

            <div class="comm">评论</div>

            <div>{{ item.commentcount }}</div>
          </div>
          <div class="cont-bar"></div>
        </div>
      </div>
    </div>

    <wd-popup v-model="show" position="bottom" :style="{ height: '180px' }">
      <div class="dynamic-details-arrangement">
        <div class="dynamic-arrangement-header">
          <div class="arrangement-header-left" @click="show = !show">取消</div>
          <div class="arrangement-header-center">回帖</div>
          <div class="arrangement-header-right" @click="sendMessage">发送</div>
        </div>
        <div class="dynamic-arrangement-body">
          <textarea ref="iptText" placeholder="请输入评论内容"></textarea>
        </div>
      </div>
    </wd-popup>
  </div>
</template>

<script>
import { mainSrc, secondUrl } from "@/utils/DynamicUrl";
export default {
  name: "GoodNightSayingDynamicDetails",
  props: {
    item: Object,
    index: Number,
    temper:Boolean
  },
  data() {
    return {
      mainSrc,
      secondUrl,
      count: 0,
      isShowLay: false,
      showCt: 0,
      isShowHeader: false,
      isPraise: false,
      show: false,
    };
  },

  created(){
    this.show=this.temper;
    console.log(this.temper)
  },

  methods: {
    JumpRouter() {
      this.$router.replace({ path: "/dynamic" });

    },
    showPicture(val) {
      this.showCt = val;
      this.isShowLay = true;
    },
    showElement() {
      this.isShowLay = !this.isShowLay;
    },
    praiseAdd() {
      if (!this.isPraise) {
       
        this.$refs.touch.src = require("../../assets/image/user-icon/dz.png");
        this.isPraise = true;
      } else {
       
        this.$refs.touch.src = require("../../assets/image/user-icon/like.png");
        this.isPraise = false;
      }
    },
    sendMessage() {
      if (!this.$refs.iptText.value) {
        this.$toast({
          msg: "评论内容不能为空",
          duration: 3000,
          position: "bottom",
        });
      }
    },
  },

};
</script>

<style lang="scss" scoped>
.dynamic-details {
  position: fixed;
  left: 0px;
  right: 0px;
  height: 100vh;
  background: #eee;
  z-index: 900;

  .dynamic-details-header {
    display: flex;
    align-items: center;
    height: 40px;
    background: #fff;

    .dynamic-details-header-arrow {
      width: 30px;
      height: 40px;
      line-height: 40px;
      text-align: center;

      .icon-arrow-left {
        font-size: 23px;
        color: #666;
      }
    }

    .dynamic-details-header-text {
      flex: 1;
      padding-right: 15px;
      text-align: center;
      color: orange;
    }
  }

  .dynamic-details-body {
    width: 100vw;
    height: 100vh;
    overflow: scroll;
    padding-bottom: 100px;

    &::-webkit-scrollbar {
      width: 0px;
    }

    .dynamic-body-content {
      width: 100%;
      margin-top: 10px;
      background: #fff;
      padding-bottom: 20px;

      .dynamic-body-header {
        display: flex;
        align-items: center;
        height: 55px;
        padding: 15px;

        .body-header-logo {
          width: 40px;
          height: 40px;
          margin-right: 7px;
          overflow: hidden;
          border-radius: 999px;

          img {
            display: block;
            width: 100%;
          }
        }
        .body-header-items {
          display: flex;
          flex-direction: column;
          justify-content: center;

          .body-header-text {
            font-size: 14px;
            margin-bottom: 5px;
          }

          .body-header-times {
            font-size: 13px;
            color: #777;
          }
        }
      }

      .dynamic-body-image {
        box-sizing: border-box;
        margin: 0px 15px;
        width: calc(100% - 30px);
        overflow: hidden;

        img {
          display: block;
          width: 100%;
          height: 240px;
        }
      }

      .body-header-img-lay {
        position: fixed;
        left: 0px;
        right: 0px;
        top: 0px;
        bottom: 0px;
        z-index: 888;
        background: black;

        .body-lay-header {
          display: flex;
          position: fixed;
          justify-content: space-between;
          left: 0px;
          right: 0px;
          height: 40px;

          .lay-header-left {
            display: flex;
            align-items: center;

            .lay-header-arrow {
              width: 40px;
              height: 40px;
              line-height: 40px;
              text-align: center;

              .icon-zuojiantou {
                font-size: 23px;
                font-weight: bold;
                color: #777;
              }
            }
            .lay-header-text {
              color: #777;
              margin-left: 10px;
            }
          }

          .lay-header-right {
            width: 40px;
            height: 40px;
            line-height: 40px;
            text-align: center;

            a {
              text-decoration: none;
            }

            .icon-download {
              color: #777;
              font-size: 30px;
            }
          }
        }

        .body-lay-img {
          display: flex;
          justify-content: center;
          align-items: center;
          width: 100%;
          height: 100vh;

          .lay-ig {
            width: 100%;

            img {
              display: block;
              width: 100%;
            }
          }
        }
      }

      .dynamic-body-words {
        margin: 0px 15px;
        line-height: 30px;
        font-size: 14px;
      }

      .dynamic-body-footer {
        display: flex;
        position: relative;
        align-items: center;
        width: 100%;
        padding-bottom: 10px;
        border-top: 1px solid #ccc;
        border-bottom: 5px dashed #eee;

        .body-footer-thumbs {
          flex: 1;
          display: flex;
          justify-content: center;
          align-items: center;
          height: 30px;
          font-size: 13px;
          color: #777;
          text-align: center;
          img {
            width: 16px;
            height: 16px;
          }
          .touch-btn {
            margin-left: 3px;
          }
        }
        .body-footer-comment {
          flex: 1;
          display: flex;
          justify-content: center;
          align-items: center;
          height: 30px;
          font-size: 13px;
          color: #777;
          text-align: center;

          img {
            width: 16px;
            height: 16px;
          }

          .comm {
            margin: 5px;
          }
        }
        .cont-bar {
          position: absolute;
          left: 0px;
          right: 0px;
          width: 1px;
          height: 22px;
          top: 4px;
          margin: auto;
          background: #ccc;
        }
      }
    }
  }
}
.dynamic-arrangement-header {
  display: flex;
  justify-content: space-between;
  align-items: center;
  height: 40px;
  background: #ccc;

  .arrangement-header-left {
    color: #777;
    margin-left: 15px;
    font-size: 15px;
  }
  .arrangement-header-center {
    color: #333;
  }
  .arrangement-header-right {
    color: #777;
    margin-right: 15px;
    font-size: 15px;
  }
}
.dynamic-arrangement-body {
  box-sizing: border-box;
  width:100%;
  height:140;
  padding:14px 0px;

  textarea {
    box-sizing: border-box;
    width: 100%;
    height:110px;
    border: none;
    padding:0px 15px;
    outline: none;
    color: #666;

    &::placeholder {
      font-size: 14px;
    }
  }
}
</style>