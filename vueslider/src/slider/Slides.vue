<template>
  <!-- mode:out-in上一個圖片先漸變離開，下一張圖片再漸變出現  -->
  <transition-group name="fade" mode="out in" tag="div" id="slides">
    <div class="slideImage" :style="setBackgroundImage" :key="images.id"></div>
  </transition-group>
</template>

<script>
export default {
  props: ["images"],
  computed: {
    setBackgroundImage() {
      return {
        backgroundImage: "url(" + this.images.url + ")"
      };
    }
  }
};
</script>

<style>
.fade-enter-active {
  animation-name: fadeEnter;
  animation-duration: 1s;
  animation-iteration-count: 1;
}

.fade-move {
  transition: all 0.5s;
}

.fade-leave-active {
  animation-name: fadeLeave;
  animation-duration: 0.5s;
  animation-iteration-count: 1;
  position: absolute;
  background: #000;
}

#slides .slideImage {
  margin: 0 auto;
  background-position: center;
  background-repeat: no-repeat;
  background-size: cover;
  filter: drop-shadow(-5px 5px 4px grey);
  border: 0.5rem solid white;
  height: 50vh;
  width: 60%;
}

@media screen and (max-width: 1034px) {
  #slides .slideImage {
    width: 70%;
    height: 40vh;
  }
}

@keyframes fadeEnter {
  from {
    opacity: 0;
  }

  to {
    opacity: 1;
  }
}

@keyframes fadeLeave {
  from {
    opacity: 1;
  }

  to {
    opacity: 0.3;
  }
}
</style>
