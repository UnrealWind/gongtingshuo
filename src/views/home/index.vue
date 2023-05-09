<template>
  <div class="container">
    <div class="music" :class="cus.music ? '' : 'music-stop'" @click.stop="changeMusic">
      <img src="../../assets/images/music.png" />
    </div>
    <audio preload autoplay loop id="vd">
      <source src="../../assets/test.mp3" type="audio/mpeg" />
    </audio>
    <!--    home page-->
    <template v-if="cus.current == 'home'">
      <div class="main-container" @click="goIndex">
        <div class="scroll">
          <img class="home" src="../../assets/images/home-font.png" />
        </div>
      </div>
    </template>
    <template v-if="cus.current == 'index'">
      <div class="index-page">
        <img style="position: absolute" class="index1" src="../../assets/images/index1.png" />
        <img style="position: absolute" class="index2" src="../../assets/images/index2.png" />
        <img style="position: absolute" class="index3" src="../../assets/images/index3.png" />
        <div class="jump-box">
          <div @click="goPage(1)"></div>
          <div></div>
          <div></div>
          <div></div>
          <div></div>
        </div>
      </div>
    </template>

    <template v-if="cus.current == 'page1'">
      <div class="index-page"> </div>
    </template>
  </div>
</template>

<script setup lang="ts">
  // @ts-nocheck
  // todo ts校验没改，任务太重了
  import { ref, onMounted } from 'vue';
  import {} from 'vant';
  import anime from 'animejs/lib/anime.es.js';

  let cus: any = ref({
    music: false,
    current: 'home',
  });

  function changeMusic() {
    cus.value.music = !cus.value.music;
    let music = document.getElementById('vd');
    console.log(cus.value.music);
    !cus.value.music ? music.pause() : music.play();
  }

  function goPage(index) {
    cus.value.current = `page${index}`;
  }

  function goIndex() {
    cus.value.current = 'index';
    changeMusic();
    setTimeout(() => {
      anime({
        targets: '.index1',
        translateX: -200,
      });
      anime({
        targets: '.index2',
        translateX: 400,
      });
      anime({
        targets: '.index3',
        translateX: 400,
        duration: 1500,
      });
    }, 500);
  }

  onMounted(() => {
    // https://animejs.com/documentation/#cssProperties
    anime({
      targets: '.home',
      translateX: -200,
    });
    let music = document.getElementById('vd');
    if (music.paused) {
      music.play();
    }
  });
</script>

<style lang="less" scoped>
  .container {
    display: flex;
    flex-direction: column;
    width: 100%;
    height: 100%;
  }
  .home {
    position: relative;
    right: -200px;
  }
  .index1 {
    position: relative;
    right: -200px;
  }
  .index2,
  .index3 {
    position: relative;
    left: -400px;
  }
  .music {
    z-index: 99;
    border: 2px solid #ffffff;
    width: 35px;
    height: 35px;
    border-radius: 50px;
    position: fixed;
    right: 20px;
    top: 10px;
    animation: rotate 6s linear infinite;
  }
  .music-stop {
    animation: none;
  }
  @keyframes rotate {
    0% {
      transform: rotateZ(0deg);
      /*从0度开始*/
    }
    100% {
      transform: rotateZ(360deg);
      /*360度结束*/
    }
  }
  .main-container {
    flex: 1;
    position: relative;
    background-image: url('../../assets/images/home-bg.jpg');
    background-size: cover;
  }

  .index-page {
    flex: 1;
    position: relative;
    background-image: url('../../assets/images/index-bg.jpg');
    background-size: cover;
    .jump-box {
      position: absolute;
      top: 200px;
      width: 100%;
      > div {
        height: 50px;
        width: 100%;
        margin-top: 30px;
      }
    }
  }
</style>
