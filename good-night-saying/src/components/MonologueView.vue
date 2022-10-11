<template>
  <div class="monologue">
    <div class="monologue-header">
      <div class="monologue-header-arrow" @click="ListenEventDy">
        <span class="iconfont icon-arrow-left"></span>
      </div>
      <div class="monologue-header-text">写独白</div>
      <div class="monologue-header-announce">发布</div>
    </div>
    <div class="monologue-body">
      <div class="monologue-body-area">
        <textarea placeholder="有什么新鲜事想告诉大家呢?"></textarea>
        <label>
          <div class="upload-image" ref="img">
            <input type="file" id="file-img" @change="uploadData" ref="file" />
          </div>
        </label>
      </div>
    </div>
    
  </div>
</template>

<script>
export default {
  methods: {
    uploadData() {
      let fram = new FileReader();
      let fr = this.$refs.file.files[0];
      fram.readAsDataURL(fr);
      const self = this;
      fram.onload = function () {
        self.$refs.img.style.backgroundImage = `url(${this.result})`;
      };
    },
    ListenEventDy(){
        this.$emit('ListenEventDy');
    }
  },
};
</script>

<style lang="scss" scoped>
.monologue {
  position: fixed;
  left: 0px;
  right: 0px;
  top: 0px;
  bottom: 0px;
  z-index: 933;
  background: #eee;

  .monologue-header {
    display: flex;
    justify-content: space-between;
    align-items: center;
    width: 100%;
    height: 40px;

    .monologue-header-arrow {
      width: 30px;
      height: 40px;
      line-height: 40px;
      text-align: center;

      .icon-arrow-left {
        font-size: 23px;
        color: #666;
      }
    }

    .monologue-header-text {
      color: orange;
    }

    .monologue-header-announce {
      width: 40px;
      height: 40px;
      line-height: 40px;
      text-align: left;
      font-size: 15px;
      color: orange;
    }
  }

  .monologue-body-area {
    position: relative;
    width: 100%;
    height: 230px;
    background: #fff;

    textarea {
      box-sizing: border-box;
      width: 100%;
      height: 130px;
      padding: 15px;
      overflow: scroll;
      margin: 0px;
      outline: none;
      border: none;
      background: none;
      &::-webkit-scrollbar {
        width: 0px;
      }
    }

    .upload-image {
      width: 80px;
      height: 80px;
      overflow: hidden;
      margin-left: 20px;
      background-image: url("../assets/image/user-icon/publish.png");
      background-size: cover;
      background-position: center;
      background-repeat: no-repeat;

      #file-img{
        display: none;
      }
    }
  }
}



</style>