<template>
  <div class="title" :id="id">
    {{ text }}
  </div>
</template>

<script>
export default {
  name: "Title",
  props: {
    text: String,
    id: String,
  },
};
</script>

<style lang="scss" scoped>
@import "plumber-sass/_plumber.scss";
@import "@/constants/_style.scss";

.title {
  @include plumber(
    $grid-height: $grid,
    $baseline: 0.1,
    $font-size: 2.25,
    $line-height: 3,
    $leading-top: 0,
    $leading-bottom: 0
  );

  font-family: soleil;
  // text-transform: uppercase;
}
</style>
