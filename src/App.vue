<template>
  <div id="app">
    <Home />
  </div>
</template>

<script>
import Home from "./views/Home";

export default {
  name: "App",
  components: { Home },
};
</script>

<style lang="scss">
@import "plumber-sass/_plumber.scss";
@import "constants/_style.scss";

* {
  box-sizing: border-box;
}

body,
html {
  margin: 0;
  padding: 0;
  font-family: jaf-lapture, sans-serif;
  width: 100vw;
  overflow-x: hidden;
}

body {
  background-color: transparent;
  // background-image: linear-gradient(
  //     0deg,
  //     rgba(0, 0, 0, 0.3) 0%,
  //     rgba(0, 0, 0, 0.3) 1%,
  //     transparent 2%,
  //     transparent 49%,
  //     rgba(0, 0, 0, 0.3) 50%,
  //     rgba(0, 0, 0, 0.3) 51%,
  //     transparent 52%
  //   ),
  //   linear-gradient(
  //     90deg,
  //     rgba(0, 0, 0, 0.3) 0%,
  //     rgba(0, 0, 0, 0.3) 1%,
  //     transparent 2%,
  //     transparent 49%,
  //     rgba(0, 0, 0, 0.3) 50%,
  //     rgba(0, 0, 0, 0.3) 51%,
  //     transparent 52%
  //   );
  height: 100%;
  background-size: 32px 32px;
  overflow-x: none;
  font-variant-numeric: oldstyle-nums;
}

#app {
  width: calc(#{$grid} * #{$block-width} * 4 + #{$grid} * #{$gutter-width} * 3);
  margin: calc(#{$grid} * #{$gutter-width});
  overflow-x: none;
}

.w1 {
  width: calc(#{$grid} * #{$block-width} * 1 + #{$grid} * #{$gutter-width} * 0);
}

.w1p {
  width: calc(
    #{$grid} * #{$block-width} * 1.5 + #{$grid} * #{$gutter-width} * 0
  );
}

.w2 {
  width: calc(#{$grid} * #{$block-width} * 2 + #{$grid} * #{$gutter-width} * 1);
}

.h2 {
  height: calc(
    #{$grid} * #{$block-width} * 2 + #{$grid} * #{$gutter-width} * 1
  ) !important;
}

.h3 {
  // this isn't really h3
  height: calc(#{$grid} * 36) !important;
}

.h4 {
  height: calc(
    #{$grid} * #{$block-width} * 4 + #{$grid} * #{$gutter-width} * 3
  ) !important;
}

.w2p {
  width: calc(
    #{$grid} * #{$block-width} * 2.5 + #{$grid} * #{$gutter-width} * 1
  );
}

.w3 {
  width: calc(#{$grid} * #{$block-width} * 3 + #{$grid} * #{$gutter-width} * 2);
}

.w3p {
  width: calc(
    #{$grid} * #{$block-width} * 3.5 + #{$grid} * #{$gutter-width} * 2
  );
}

.w4 {
  width: calc(#{$grid} * #{$block-width} * 4 + #{$grid} * #{$gutter-width} * 3);
}

.w4p {
  width: calc(
    #{$grid} * #{$block-width} * 4.5 + #{$grid} * #{$gutter-width} * 3
  );
}

.l1 {
  margin-left: calc(
    #{$grid} * #{$block-width} * 1 + #{$grid} * #{$gutter-width} * 1
  );
}

.l2 {
  margin-left: calc(
    #{$grid} * #{$block-width} * 2 + #{$grid} * #{$gutter-width} * 2
  );
}

.l3 {
  margin-left: calc(
    #{$grid} * #{$block-width} * 3 + #{$grid} * #{$gutter-width} * 3
  );
}

.w2 .float {
  @include plumber(
    $grid-height: $grid,
    $baseline: 0.25,
    $font-size: 0.75,
    $line-height: 1,
    $leading-top: 0,
    $leading-bottom: 1
  );

  font-family: jaf-lapture-caption, sans-serif;
  position: absolute;
  width: calc(#{$grid} * #{$block-width} * 2 + #{$grid} * #{$gutter-width} * 1);
  top: calc(#{$grid} * #{$gutter-width} * 3.5);
  left: calc(#{$grid} * #{$block-width} * 2 + #{$grid} * #{$gutter-width} * 2);
}

.float.left {
  font-family: jaf-lapture-caption, sans-serif;
  position: absolute;
  width: calc(#{$grid} * #{$block-width} * 1 + #{$grid} * #{$gutter-width} * 0);
  top: calc(#{$grid} * #{$gutter-width} * 3);
  left: calc(
    -1 * #{$grid} * #{$block-width} * 1 - #{$grid} * #{$gutter-width} * 1
  );
}

.float.right {
  font-family: jaf-lapture-caption, sans-serif;
  position: absolute;
  width: calc(#{$grid} * #{$block-width} * 1 + #{$grid} * #{$gutter-width} * 0);
  top: calc(#{$grid} * #{$gutter-width} * 3);
  right: calc(
    -1 * #{$grid} * #{$block-width} * 1 - #{$grid} * #{$gutter-width} * 1
  );
}

.float.d0 {
  top: 12px;
}

.float.d3 {
  top: calc(#{$grid} * #{$gutter-width} * 2.5);
}

.float.d4 {
  top: calc(#{$grid} * #{$gutter-width} * 3.5);
}

.float.d6 {
  top: calc(#{$grid} * #{$gutter-width} * 5.5);
}

.share {
  display: flex;
  justify-content: space-between;

  &.align-bottom {
    align-items: flex-end;
  }
}

.p {
  @include plumber(
    $grid-height: $grid,
    $baseline: 0.25,
    $font-size: 1,
    $line-height: 2,
    $leading-top: 0,
    $leading-bottom: 1
  );

  margin-top: 0px !important;

  &-no-leading {
    @include plumber(
      $grid-height: $grid,
      $baseline: 0.25,
      $font-size: 1,
      $line-height: 1,
      $leading-top: 0,
      $leading-bottom: 1
    );
  }

  text-align: justify;
  margin-top: 16px;
  margin-bottom: 16px;
  position: relative;
  hyphens: auto;
}

.caption {
  @include plumber(
    $grid-height: $grid,
    $baseline: 0.25,
    $font-size: 0.75,
    $line-height: 1,
    $leading-top: 0,
    $leading-bottom: 1
  );

  font-family: jaf-lapture-caption, sans-serif;
}

img {
  width: 100%;
  margin-bottom: 0px;
  padding-bottom: 0px;
}

@media (max-width: 928px) {
  .w1,
  .w3,
  .w4,
  .w4p,
  .w3p,
  .w2p,
  .w1p,
  .l1,
  .l2,
  .l3,
  .h2,
  .h3,
  .h4,
  .float {
    margin-left: 0px !important;
    max-width: 100% !important;
    position: relative !important;
    left: 0px !important;
    top: 0px !important;
    height: auto !important;
  }

  img {
    max-width: calc(100vw - 2 * #{$gutter-width} * #{$grid});
  }

  #app {
    width: calc(100% - 2 * #{$gutter-width} * #{$grid});
  }

  .share {
    flex-direction: column;
    align-items: flex-start !important;
  }

  .below {
    margin-bottom: 0px !important;
  }
}

a,
a:visited {
  color: black;
}
</style>
