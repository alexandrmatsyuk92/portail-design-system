<!--
  Page header

  Usage:

    <AppHeader>
      <MenuButton #left />
      Hello, World!
      <AccountMenu #right />
    </AppHeader>
-->

<template>
  <div class="app-header">
    <slot name="left"></slot>
    <div class="title">
      <slot></slot>
    </div>
    <slot name="right"></slot>
  </div>
</template>

<script>
export default {
  name: 'AppHeader',
}
</script>

<style lang="less">
@import '../styles/vars';

.app-header {
  background-color: @color-white;
  height: @app-header-height;
  box-shadow: inset 0 -1px 0 0 @color-gray-200;
  display: flex;
  align-items: stretch;

  .title {
    flex: 1 0 auto;
    .font-desktop-h-3-dark();
    padding: @app-header-title-padding;
  }

  @media @screen-small {
    height: @app-header-height-small;

    .title {
      .font-desktop-body-medium-dark();
      padding: @app-header-title-padding-small;
    }
  }
}
</style>
