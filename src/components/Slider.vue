<template>
  <div>
    <div v-for="item in pokemons"
         :key="item.id"

    >
      <transition>
        <div :style="{'background-image': `url(${item.path})`}"
             v-show="item.showed"
             class="slide"
        >
        </div>
      </transition>
    </div>
  </div>
</template>

<script>
export default {
  name: 'Slider',
  props: {
      pokemons: Array
  }
}
</script>

<style lang="scss">
@mixin square ($size) {
    width: $size;
    height: $size;
  }
@mixin center {
  position: absolute;
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%);
}
@mixin fill_image {
  background-repeat: no-repeat;
  background-position: center;
  background-size: cover;
}

.slide {
  @include center;
  position: fixed;
  @include square(264px);
  @include fill_image;
  opacity: 1;
  transition: all 1.5s ease-out 0s;
}
.v-enter {
  left: 150%;
}
.v-enter-to, v-leave {
  left: 50%;
}
.v-leave-active {
  transition: all .5s ease-out 0s;
}
.v-leave-to {
  @include square(0px);
  opacity: 0;
}

</style>
