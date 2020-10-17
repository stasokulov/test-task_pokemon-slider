<template>
  <div>
    <div v-for="item in pokemons"
         :key="item.id"
         :style="{'background-image': `url(${item.path})`}"
         :class="[{'slide-showed': item.showed}, {'slide-hided': item.hided}]"
         class="slide"
    >
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
  left: 150%;
  opacity: 1;
  transition: all 1.5s ease-out 0s;
}
.slide-showed {
  left: 50%;
}
.slide-hided {
  transition: all .5s ease-out 0s;
  @include square(0px);
  opacity: 0;
}
</style>
