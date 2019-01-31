<template>
  <div id="slider">
    <div class="arrowLeft" @click="arrowLeft()"></div>
    <!-- 子組件 -->
    <Slides :images="images[chooseImage]"/>
    <div class="arrowRight" @click="arrowRight()"></div>
    <div class="squares">
      <div @click="square(image.id)" v-for="image in images" :key="image.id" :style="setColor"></div>
    </div>
  </div>
</template>

<script>
import Slides from "./Slides.vue";
export default {
  components: {
    Slides
  },
  data() {
    return {
      images: [
        {
          id: 0,
          url: "http://18.225.4.201:8080/static/image1.jpeg",
          title: "one"
        },
        {
          id: 1,
          url: "http://18.225.4.201:8080/static/image2.jpg",
          title: "two"
        },
        {
          id: 2,
          url: "http://18.225.4.201:8080/static/image3.jpeg",
          title: "three"
        },
        {
          id: 3,
          url: "http://18.225.4.201:8080/static/image4.jpeg",
          title: "four"
        },
        {
          id: 4,
          url: "http://18.225.4.201:8080/static/image5.jpeg",
          title: "five"
        }
      ],
      chooseImage: 0,
      intervalObject: null
    };
  },

  methods: {
    //目前點到的變成灰色，可避免重複code，之後要改顏色較方便
    chosenBlock(id) {
      this.$el.children[3].children[id].style.backgroundColor = "grey";
    },
    //其他的初始化為黑色
    initializeBlock(id) {
      this.$el.children[3].children[id].style.backgroundColor = "black";
    },
    //click事件傳入目前的image.id
    square(id) {
      //讓其他沒有被點到的回到黑色
      for (let i = 0; i < this.images.length; i++) {
        this.initializeBlock(i);
      }
      this.chosenBlock(id);
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
      //   let self = this;
      //繼續回到原本輪播狀態，倒著播
      this.intervalObject = setInterval(() => {
        this.moveLeft();
      }, 4000);
    },
    //點擊右箭頭，跑到下一個
    arrowRight() {
      //清除事件
      clearInterval(this.intervalObject);
      this.moveRight();

      this.intervalObject = setInterval(() => {
        this.moveRight();
      }, 4000);
    },
    moveRight() {
      for (let i = 0; i < this.images.length; i++) {
        this.initializeBlock(i);
      }
      let imageIndex = this.chooseImage;
      imageIndex++;
      //如果大於最大的index，返回到第一個
      if (imageIndex > this.images.length - 1) {
        imageIndex = 0;
      }
      this.chooseImage = imageIndex;
      this.chosenBlock(imageIndex);
    },
    moveLeft() {
      for (let i = 0; i < this.images.length; i++) {
        this.initializeBlock(i);
      }
      let imageIndex = this.chooseImage;
      imageIndex--;
      if (imageIndex < 0) {
        //如果小於0，返回到最後一個
        imageIndex = this.images.length - 1;
      }
      this.chooseImage = imageIndex;
      this.chosenBlock(imageIndex);
    }
  },
  created() {
    //自動輪播，每4秒輪播
    this.intervalObject = setInterval(() => {
      this.moveRight();
    }, 4000);
  }
};
</script>

<style>
/* google font */
* {
  font-family: "Raleway", sans-serif;
}
#slider {
  position: relative;
}
#slider .arrowLeft,
#slider .arrowRight {
  position: absolute;
  top: 50%;
  border: 30px solid transparent;
  cursor: pointer;
}

#slider .arrowLeft {
  border-right-color: black;
  z-index: 1;
  filter: drop-shadow(5px 5px 4px grey);
}

#slider .arrowRight {
  border-left-color: black;
  right: 0;
  filter: drop-shadow(-5px 5px 4px grey);
}

#slider .squares {
  position: absolute;
  display: flex;
  justify-content: center;
  margin: 0 auto;
  right: 0;
  left: 0;
  bottom: 2rem;
}

#slider .squares div {
  width: 1.5rem;
  height: 1.5rem;
  border-radius: 50%;
  background-color: black;
  text-align: center;
  margin: 0 0.75rem;
  color: black;
  cursor: pointer;
}
</style>
