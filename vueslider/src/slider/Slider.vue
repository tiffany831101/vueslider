<template>
  <div id="slider">
    <div class="arrowLeft" @click="arrowLeft()">
      <i class="fas fa-chevron-left"></i>
    </div>
    <!-- 子組件傳入目前的圖片，chooseImage代表目前的index -->
    <Slides :images="images[chooseImage]"/>
    <div class="arrowRight" @click="arrowRight()">
      <i class="fas fa-chevron-right"></i>
    </div>
    <!-- 下 -->
    <div class="squares">
      <div
        @click="square(image.id)"
        v-for="image in images"
        :key="image.id"
        :style="changeColor(image.id)"
      >
        <!-- hover過去會產生縮圖 -->
        <img class="thumbnail" :src="image.url" alt>
      </div>
    </div>
  </div>
</template>

<script>
import Slides from "./Slides.vue";
export default {
  components: {
    Slides
  },
  computed: {
    thumbnail() {
      return {
        backgroundImage: "url(" + image.url + ")"
      };
    }
  },
  data() {
    return {
      images: [],
      //目前選到的index
      chooseImage: 0,
      //setInterval
      intervalObject: null
    };
  },

  methods: {
    //目前的image會透明度變1
    changeColor(id) {
      //如果當前的id跟thumbnail的id是同一個，則透明度變1
      if (this.chooseImage == id) {
        return {
          opacity: 1
        };
      }
    },
    square(id) {
      //讓chooseImage變成目前點到的id，以傳入子組件
      this.chooseImage = id;
      //清除interval事件
      clearInterval(this.intervalObject);
      //重新繼續輪播
      this.intervalObject = setInterval(() => {
        this.moveRight();
      }, 4000);
    },
    //點擊左箭頭，跑回上一個
    arrowLeft() {
      //先清除事件
      clearInterval(this.intervalObject);
      //執行回上一個
      this.moveLeft();
      //繼續回到原本輪播狀態，倒著播
      this.intervalObject = setInterval(() => {
        this.moveLeft();
      }, 4000);
    },
    //點擊右箭頭，跑到下一個
    arrowRight() {
      //清除事件
      clearInterval(this.intervalObject);
      // 執行了向右一個之後
      this.moveRight();
      //繼續setInterval
      this.intervalObject = setInterval(() => {
        this.moveRight();
      }, 4000);
    },
    moveRight() {
      let imageIndex = this.chooseImage;
      imageIndex++;
      //如果大於最大的index，返回到第一個
      if (imageIndex > this.images.length - 1) {
        imageIndex = 0;
      }
      this.chooseImage = imageIndex;
    },
    moveLeft() {
      let imageIndex = this.chooseImage;
      imageIndex--;
      if (imageIndex < 0) {
        //如果小於0，返回到最後一個
        imageIndex = this.images.length - 1;
      }
      this.chooseImage = imageIndex;
    }
  },
  //template已經掛載，data裡面還沒有掛載到template
  created() {
    //自動輪播，每4秒輪播
    this.intervalObject = setInterval(() => {
      this.moveRight();
    }, 4000);
  },
  mounted() {
    axios
      .get(
        "https://newsapi.org/v2/everything?q=social&page=1&apiKey=00bbb1d312884ea59975c695f7c687f1"
      )
      .then(response => {
        this.images = response.data.articles
          .filter((image, index) => index < 5)
          .map((image, index) => ({
            id: index,
            url: image.urlToImage
          }));
      })
      .catch(error => {
        console.log(error);
      });
  }
};
</script>

<style>
* {
  font-family: "Playfair Display", serif;
  box-sizing: border-box;
}
#slider {
  margin-top: 2em;
  position: relative;
}

#slider .arrowLeft,
#slider .arrowRight {
  position: absolute;
  top: 50%;
  color: black;
  font-size: 3em;
  cursor: pointer;
  transform: translateY(-50%);
}

#slider .arrowLeft {
  left: 10%;
  z-index: 1;
  filter: drop-shadow(5px 5px 4px grey);
}

#slider .arrowRight {
  right: 10%;
  filter: drop-shadow(-5px 5px 4px grey);
}

#slider .squares {
  width: 60%;
  display: flex;
  flex-wrap: wrap;
  justify-content: space-around;
  margin: 0 auto;
  margin-top: 2em;
  right: 0;
  left: 0;
}

#slider .squares div {
  position: relative;
  margin: 0 0.75rem;
  cursor: pointer;
  opacity: 0.3;
}

.thumbnail {
  width: 100px;
  height: 75px;
  border: 0.5rem solid black;
}
/* 在1034以下，圖片消失，變成圓圈 */
@media screen and (max-width: 1034px) {
  .thumbnail {
    display: none;
  }

  #slider .squares div {
    width: 1.5rem;
    height: 1.5rem;
    border-radius: 50%;
    background-color: black;
  }
}
</style>


