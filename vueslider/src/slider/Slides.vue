<template>
  <!-- {{images.url}} -->
  <transition-group name="fade" mode="out in" tag="div" id="slides">
    <div class="slideImage" :style="setBackgroundImage" :key="images.id">
      <div class="slideTitle"></div>
    </div>
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
#slides .slideImage {
  height: 100vh;
  width: 100%;
  background-position: center;
  background-repeat: no-repeat;
  background-size: cover;
}

#slides .slideImage .slideTitle {
  color: red;
  font-weight: bold;
  font-size: 40px;
  height: 100vh;
  display: flex;
  justify-content: center;
  align-items: center;
}

.fade-enter-active {
  animation-name: fadeEnter;
  animation-duration: 1s;
  animation-iteration-count: 1;
}

.fade-move {
  transition: all 1s;
}

.fade-leave-active {
  animation-name: fadeLeave;
  animation-duration: 1s;
  animation-iteration-count: 1;
  position: absolute;
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
    opacity: 0;
  }
}
</style>
