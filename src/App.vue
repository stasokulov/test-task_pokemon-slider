<template>
  <div id="app">
    <div class="picachu">
      <div class="picachu__image picachu__image-1"
           :class='{picachu__image_show: readyState && !workSliderState}'>
      </div>
      <div class="picachu__image picachu__image-2"
           :class='{picachu__image_show: readyState && workSliderState && counter%2 === 0}'>
      </div>
      <div class="picachu__image picachu__image-3"
           :class='{picachu__image_show: readyState && workSliderState && counter%2 !== 0}'>
      </div>
    </div>
    <div class="top">
      <div class="title__wrap">
        <div class="title" :class='{title_show: readyState}'>
          <div class="title__letter">
            <span class="title__letter-back">P</span>
            <span class="title__letter-front">P</span>
          </div>
          <div class="title__letter">
            <span class="title__letter-back">o</span>
            <span class="title__letter-front">o</span>
          </div>
          <div class="title__letter">
            <span class="title__letter-back">K</span>
            <span class="title__letter-front">K</span>
          </div>
          <div class="title__letter">
            <span class="title__letter-back">e</span>
            <span class="title__letter-front">e</span>
          </div>
          <div class="title__letter">
            <span class="title__letter-back">M</span>
            <span class="title__letter-front">M</span>
          </div>
          <div class="title__letter">
            <span class="title__letter-back">o</span>
            <span class="title__letter-front">o</span>
          </div>
          <div class="title__letter">
            <span class="title__letter-back">N</span>
            <span class="title__letter-front">N</span>
          </div>

        </div>
      </div>
    </div>
    <div class="bottom">
      <div tabindex="1"
           class="start"
           :class='{start_ready: readyState && !workSliderState}'
           @click="startSlider()"
      >
        <span class="start__text"
              :class='{start__text_show: readyState}'
              v-if="!workSliderState"
        >start</span>
        <div class="start__start-icon" v-if="!workSliderState || pause"></div>
        <div class="start__pause-icon" v-if="workSliderState && !pause"></div>
      </div>
    </div>
    <div class="spinner" v-if="workSpinnerState">
      <div class="spinner__bg spinner__top"></div>
      <div class="spinner__bg spinner__bottom"></div>
      <div class="spinner__half-circle spinner__progress spinner__progress-1"></div>
      <div class="spinner__half-circle spinner__progress spinner__progress-2"></div>
    </div>
    <div class="pokebol" :class='{pokebol_show: readyState, pokebol__opened: workSliderState}'>
      <div class="pokebol__top">
        <div class="pokebol__open-lock pokebol__open-lock_top"
             :class="{'pokebol__open-lock_opened': workSliderState}"
        ></div>
      </div>
      <div class="pokebol__bottom">
        <div class="pokebol__open-lock pokebol__open-lock_bottom"
             :class="{'pokebol__open-lock_opened': workSliderState}"
        ></div>
      </div>
      <div class="pokebol__lock" :class="{pokebol__lock_opened: workSliderState}">
        <div class="pokebol__lock-top"></div>
        <div class="pokebol__lock-bottom"></div>
        <div class="pokebol__lock-center"></div>
      </div>
      <div class="pokebol__ray"></div>
    </div>
    <div class="line" :class='{line__show: workSliderState}'></div>
    <Slider :pokemons="pokemons" />
  </div>
</template>

<script>
import Slider from './components/Slider.vue'

export default {
  name: 'App',
  components: {
      Slider
  },
  mounted() {
      setTimeout(()=>this.readyState = true,1000)
  },
  data() {
    return {
      readyState: false,
      workSliderState: false,
      pokemons: [
            {
                id: 1,
                path: require('./assets/img/pokemons/pokemon_1.svg'),
                showed: false,
                hided: false
            },
            {
                id: 2,
                path: require('./assets/img/pokemons/pokemon_2.svg'),
                showed: false,
                hided: false
            },
            {
                id: 3,
                path: require('./assets/img/pokemons/pokemon_3.svg'),
                showed: false,
                hided: false
            }
        ],
      counter: 0,
      workSpinnerState: false,
      pause: false,
      slideShow: false
    }
  },
  methods: {
    startSlider: function () {
        if (!this.workSliderState) {
            this.workSliderState = true
            this.pokemons.forEach(item => {
                item.showed = false
                item.hided = false
            })
            setTimeout(this.goSlides, 2000)
        } else if (this.pause) {
            this.pause = false
            if (!this.slideShow) {
                this.goSlides()
            }
        } else {
            this.pause = true
        }

    },
    goSlides: function () {
      this.pokemons[this.counter].showed = true
      this.slideShow = true
      setTimeout(() => this.workSpinnerState = true, 0)
      setTimeout(() => {
          this.pokemons[this.counter].showed = false
          // this.pokemons[this.counter].hided = true
          this.workSpinnerState = false
          this.slideShow = false
        if (this.counter < this.pokemons.length - 1) {
            this.counter++
            if (!this.pause) {
                this.goSlides()
            }
        } else {
            this.endSlides()
        }
      }, 4000)
    },
    endSlides: function () {
        this.workSliderState = false
        this.pause = false
        this.counter = 0
    }
  }
}
</script>

<style lang="scss">
@import url('https://fonts.googleapis.com/css2?family=Inter:wght@900&display=swap');
@font-face {
  font-family:"Pokemon Solid Normal";
  src: url("assets/fonts/pokemonsolid.eot?") format("eot"),
       url("assets/fonts/pokemonsolid.woff") format("woff"),
       url("assets/fonts/pokemonsolid.ttf") format("truetype"),
       url("assets/fonts/pokemonsolid.svg#PokemonSolidNormal") format("svg");
  font-weight:normal;
  font-style:normal;
}

$bg_color: #E33F3F;
$bg-light_color: #FFF;
$main_color: #d03131;
$yellow_color: #FCD83D;
$blue_color: #005BD4;
$light-blue_color: #01D1ED;

@mixin center {
  position: absolute;
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%);
}
@mixin square ($size) {
  width: $size;
  height: $size;
}
@mixin fill_image {
  background-repeat: no-repeat;
  background-position: center;
  background-size: cover;
}
@mixin half_height {
  width: 100%;
  height: 50%;
}

body {
  margin: 0;
}
#app {
  .top, .bottom {
    height: 50vh;
  }
  .top {
    background-color: $bg_color;
  }
  .bottom {
    position: relative;
  }
  .title__wrap {
    position: absolute;
    top: 80px;
    left: 40px;
    height: 157px;
    width: 480px;
    overflow: hidden;
    .title {
      display: flex;
      position: absolute;
      top: 160px;
      left: 50%;
      transform: translateX(-50%);
      font-family:"Pokemon Solid Normal", sans-serif;
      font-style: normal;
      font-weight: 400;
      font-size: 94px;
      line-height: 132px;
      color: $yellow_color;
      transition: all 1s ease-out 0s;
      &_show {
        top: 5px;
      }
      .title__letter {
        position: relative;
        height: 560px;
        margin-right: -11px;
        .title__letter-back {
          -webkit-text-stroke: 18px $blue_color;
        }
        .title__letter-front {
          position: absolute;
          top: 0;
          left: 0;
        }
        &:nth-child(1) {
          transform: rotate(-13.67deg);
          margin-top: 12px;
        }
        &:nth-child(2) {
          transform: rotate(-9.3deg);
          margin-top: 4px;
        }
        &:nth-child(3) {
          transform: rotate(-4.93deg);
          margin-top: 2px;
        }
        &:nth-child(4) {
          transform: rotate(-0.56deg);
          margin-top: 0;
        }
        &:nth-child(5) {
          transform: rotate(4.37deg);
          margin-top: 2px;
        }
        &:nth-child(6) {
          transform: rotate(9.3deg);
          margin-top: 9px;
        }
        &:nth-child(7) {
          transform: rotate(13.67deg);
          margin-top: 14px;
        }
      }
    }
  }
  .pokebol {
    @include center;
    @include square(246px);
    left: -30%;
    transition: all 1s ease-out 0s;
    &_show {
      left: 50%;
    }
    &__top, &__bottom {
      @include fill_image;
      @include half_height;
      transition: all .5s ease-out 1s;
    }
    &__top {
      background-image: url("assets/img/pokebol/pokebol_top.svg");
      transform: translateY(3px);
    }
    &__bottom {
      background-image: url("assets/img/pokebol/pokebol_bottom.svg");
      transform: translateY(-3px);
    }
    &__lock {
      @include center;
      @include square(62px);
      transition: all .4s ease-out 1s;
      opacity: 1;
      &_opened {
        transition: all .4s ease-out 0s;
        left: 90%;
        opacity: 0;
      }
      &-top, &-bottom {
        @include fill_image;
        @include half_height;
        background-image: url("assets/img/pokebol/pokebol_lock_half_front.svg");
      }
      &-top {
        transform: rotateX(180deg);
      }
      &-center {
        @include center;
        background-image: url("assets/img/pokebol/pokebol_lock_center.svg");
        @include square(32px);
      }
    }
    &__open-lock {
      position: absolute;
      left: 50%;
      transform: translateX(-50%);
      @include square(31px);
      @include fill_image;
      background-size: contain;
      background-image: url("assets/img/pokebol/pokebol_lock_half_side.svg");
      opacity: 0;
      transition: all .5s ease-out 1s;
      &_top {
        top: 100%;
        transform: translateX(-50%)
                   translateY(calc(-100% - 2px))
                   rotateX(180deg);
      }
      &_bottom {
        transform: translateX(-50%)
                   translateY(2px)
      }
      &_opened {
        transition: all .5s ease-out 0s;
        left: 96%;
        opacity: 1;
      }
    }
    &__ray {
      position: absolute;
      top: calc(50% - 1px);
      left: 10px;
      transform: translateY(-50%);
      @include fill_image;
      background-image: url("assets/img/pokebol/pokebol_ray.svg");
      width: 455px;
      height: 57px;
      opacity: 0;
      transition: all 1s ease-out 1s;
    }
    &__opened {
      .pokebol__top {
        transition: all .5s ease-out 0s;
        transform: rotate(-3.78deg);
        transform-origin: 5%;
      }
      .pokebol__bottom {
        transition: all .5s ease-out 0s;
        transform: rotate(3.78deg);
        transform-origin: 5%;
      }
      .pokebol__ray {
        transition: all .5s ease-out 0s;
        opacity: 1;
      }
    }
  }
  .start {
    position: absolute;
    display: flex;
    align-items: center;
    justify-content: center;
    top: 60%;
    left: 50%;
    transform: translateX(-50%);
    cursor: pointer;
    font-family: 'Inter', sans-serif;
    font-style: normal;
    font-size: 44px;
    font-weight: 900;
    color: $main_color;
    box-sizing: border-box;
    width: 84px;
    height: 84px;
    border-radius: 40px;
    &:hover {
      background-color: $bg_color;
      color: $bg-light_color;
      .start__start-icon {
        border-left-color: $bg-light_color;
      }
      .start__pause-icon {
        background-color: $bg_color;
        border-color: $bg-light_color;
      }
    }
    &:focus {
      border: 4px solid $bg_color;
      outline: none;
    }
    &:active {
      @include square(70px);
      background-color: $main_color;
      color: $bg-light_color;
      border: none;
      .start__icon {
        border-left-color: $bg-light_color;
      }
    }
    &_ready {
      width: 245px;
      &:active {
        width: 207px;
      }
    }
    &__text {
      width: 0;
      overflow: hidden;
      transition: all 1s ease-out 0s;
      &_show {
        width: 107px;
      }
    }
    &__start-icon {
      width: 0;
      height: 0;
      border-style: solid;
      border-left-color: $main_color;
      border-left-width: 24px;
      border-right-width: 0;
      border-top-width: 15px;
      border-bottom-width: 15px;
      border-top-color: transparent;
      border-bottom-color: transparent;
      margin-left: 13px;
    }
    &__pause-icon {
      width: 6px;
      height: 30px;
      border-left: 9px;
      border-right: 9px;
      border-top: 0;
      border-bottom: 0;
      border-color: $bg_color;
      border-style: solid;
    }
  }
  .picachu {
    position: absolute;
    bottom: 0;
    left: 0;
    &__image {
      position: absolute;
      bottom: 0;
      left: -550px;
      background-repeat: no-repeat;
      background-position: left bottom;
      background-size: contain;
      transition: all 1s ease-out 0s;
      &_show {
        left: 0;
      }
    }
    &__image-1 {
      @include square(500px);
      background-image: url("assets/img/picachu/picachu_1.svg");
    }
    &__image-2 {
      @include square(530px);
      background-image: url("assets/img/picachu/picachu_2.svg");
    }
    &__image-3 {
      @include square(510px);
      background-image: url("assets/img/picachu/picachu_3.svg");
    }
  }
  .line {
    position: absolute;
    top: 50%;
    right: 0;
    transform: translateY(-50%);
    width: 0%;
    height: 10px;
    background-color: $light-blue_color;
    transition: all .5s ease-out 1s;
    &__show {
      width: 50%;
    }
  }
  .spinner {
    position: absolute;
    @include center;
    @include square(322px);
    background-color: gray;
    z-index: 0;
    &__bg {
      position: absolute;
      width: 100%;
      height: 50%;
    }
    &__top {
      top: 0;
      background-color: $bg_color;
      z-index: 4;
      animation: top 2s steps(1, end) 1 forwards;
    }
    &__bottom {
      bottom: 0;
      background-color: $bg-light_color;
      z-index: 2;
    }
    &__progress {
      position: absolute;
      @include square(0);
      border: solid 161px rgba(1, 209, 237, 0.1);
      border-bottom: none;
      border-radius: 161px 161px 0 0;
      transform-origin: center bottom;
      &-1 {
        transform: rotate(0deg);
        z-index: 3;
        animation: rotate-1 2s linear 1 forwards;
      }
      &-2 {
        top: 0;
        transform: rotate(180deg);
        transform-origin: center bottom;
        z-index: 1;
        animation: rotate-2 4s linear 1 forwards;
      }
    }
    @keyframes rotate-1 {
      0%   { transform: rotate(0deg); }
      100% { transform: rotate(180deg); }
    }
    @keyframes rotate-2 {
      0%, 50%   { transform: rotate(180deg); }
      100% { transform: rotate(360deg); }
    }
    @keyframes top {
      0%, 50% { z-index: 4; }
      100%    { z-index: 1; }
    }


  }

  @media (max-width: 769px) {
    .title__wrap {
      left: 50%;
      transform: translateX(-50%);
    }
    .picachu {
      &__image_show {
        &.picachu__image-1 {
          left: -90px;
        }
        &.picachu__image-2 {
          left: -140px;
        }
        &.picachu__image-3 {
          left: -155px;
        }
      }
    }
  }
}
</style>
