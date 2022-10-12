<template>
  <div class="recordsaying">
    <transition name="move">
      <router-view :item="currentItem" :index="ind" :temper="temper" />
    </transition>
    <div class="recordsaying-header">
      <div class="recordsaying-header-text">动态</div>
      <div
        class="recordsaying-header-img"
        @click="isShowMonologue = !isShowMonologue"
      >
        <img src="../../assets/image/user-icon/user_icon_feedback.png" alt="" />
      </div>
    </div>
    <div class="recordsaying-body-main">
      <div
        class="recordsaying-body-content"
        v-for="(items, index) in DynamicArr"
        :key="items.id"
      >
        <div class="recordsaying-body-header">
          <div class="body-header-logo">
            <img :src="items.headimage ? secondUrl[index] : secondUrl[7]" />
          </div>
          <div class="body-header-items" @click="JumpRouter(items, index)">
            <div class="body-header-text">{{ items.username }}</div>
            <div class="body-header-times">
              <span>{{ items.timeline }}</span>
            </div>
          </div>
        </div>
        <div class="recordsaying-body-image" @click="showPicture(index)">
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
        <div class="recordsaying-body-words" @click="JumpRouter(items, index)">
          {{ items.content }}
        </div>
        <div class="recordsaying-body-footer">
          <div class="body-footer-thumbs">
            <img src="../../assets/image/user-icon/like.png" ref="touch" />
            <div class="touch-btn">{{ items.pmax }}</div>
          </div>
          <div class="body-footer-comment">
            <img src="../../assets/image/user-icon/comment.png" />
            <div class="comm" @click="commentWord(items, index)">评论</div>
            <div>{{ items.commentcount }}</div>
          </div>
          <div class="cont-bar"></div>
        </div>
      </div>
    </div>

    <transition name="move">
      <MonologueView v-if="isShowMonologue" @ListenEventDy="ListenEventDy" />
    </transition>

    <wd-loading class="load" size="26px" v-if="!isCurrentDy"></wd-loading>
  </div>
</template>


<script>
import { mainSrc, secondUrl } from "@/utils/DynamicUrl";
import MonologueView from "@/components/MonologueView.vue";
import { debounce } from "lodash-es";

export default {
  data() {
    return {
      DynamicArr: [],
      mainSrc,
      secondUrl,
      count: 0,
      isShowLay: false,
      showCt: 0,
      isShowHeader: false,
      currentItem: {},
      ind: 0,
      temper: false,
      isShowMonologue: false,
      isCurrentDy: false,
    };
  },

  created() {
    this.getDynamicData = debounce(this.getDynamicData);
    this.getDynamicData();
  },
  mounted() {},

  methods: {
    getDynamicData() {
      this.$axios
        .get(
          "/shengcai/api/mobile/common/showFeedInfosByMeetid?userid=153225&meetid=1339&lang=0&moduletype=1&currentpage=1&maxresult=20"
        )
        .then(({ data }) => {
          this.DynamicArr = [...data.feedInfoList];
          this.isCurrentDy = true;
        });
    },
    showPicture(val) {
      this.showCt = val;
      this.isShowLay = true;
    },
    showElement() {
      this.isShowLay = !this.isShowLay;
    },
    JumpRouter(val1, val2) {
      this.$router.push({ path: "/dynamic/dynamic-details" });
      this.currentItem = val1;
      this.ind = val2;

      this.temper = false;
    },
    commentWord(v1, v2) {
      this.temper = true;
      this.$router.push({ path: "/dynamic/dynamic-details" });
      this.currentItem = v1;
      this.ind = v2;
    },
    ListenEventDy() {
      this.isShowMonologue = false;
    },
  },

  components: {
    MonologueView,
  },
};
</script>


<style lang="scss" scoped>
.recordsaying {
  box-sizing: border-box;
  width: 100%;
  height:100vh;
  background: #eee;

  .recordsaying-header {
    display: flex;
    justify-content: flex-end;
    align-items: center;
    position: fixed;
    width: 100%;
    height: 40px;
    line-height: 40px;
    font-size: 16px;
    color: orange;
    background-color: #fff;
    text-align: center;
    z-index: 6;

    .recordsaying-header-text {
      flex: 1;
      margin-left: 13px;
    }

    .recordsaying-header-img {
      width: 30px;
      height: 30px;
      margin-right: 10px;

      img {
        display: block;
        width: 100%;
      }
    }
  }

  .recordsaying-body-main {
    box-sizing: border-box;
    width: 100vw;
    height: 100vh;
    overflow: scroll;
    padding-top:40px;
    padding-bottom: 100px;

    &::-webkit-scrollbar {
      width: 0px;
    }

    .recordsaying-body-content {
      width: 100%;
      margin-top: 10px;
      background: #fff;

      .recordsaying-body-header {
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

      .recordsaying-body-image {
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

      .recordsaying-body-words {
        margin: 0px 15px;
        line-height: 30px;
        font-size: 14px;
      }

      .recordsaying-body-footer {
        display: flex;
        position: relative;
        align-items: center;
        width: 100%;
        border-top: 1px solid #ccc;

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
          top: 0px;
          bottom: 0px;
          margin: auto;
          background: #ccc;
        }
      }
    }
  }
}

.move-enter,
.move-leave-to {
  transform: translateX(100%);
}
.move-enter-active,
.move-leave-active {
  transition: all 0.2s linear;
}
.move-enter-to,
.move-leave {
  transform: translateX(0px);
}

.load {
  position: fixed;
  top: 50px;
  left: 0px;
  right: 0px;
  margin: 0px auto;
}
</style>