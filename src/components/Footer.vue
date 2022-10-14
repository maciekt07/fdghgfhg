<template>
  <footer>
    Made with <fa class="heart" icon="heart" /> By
    <a target="_blank" href="https://github.com/maciekt07">
      <!-- <fa :icon="['fab', 'github']" />&nbsp; -->
      github.com/maciekt07</a
    >
  </footer>
</template>
<style lang="scss" scoped>
@use "../style.scss" as *;
footer {
  position: fixed;
  left: 0;
  bottom: 0;
  margin-bottom: 20px;
  width: 100%;
  color: white;
  text-align: center;
  font-size: 18px;
  @include footerSelection();
}
.heart {
  transition: 0.3s;
  &:hover {
    color: $linkColor;
  }
}

a:hover::after {
  transform: scaleX(1);
  transform-origin: bottom left;
}

a {
  cursor: pointer;
  transition: 0.4s all;
  color: $linkColor;
  display: inline-block;
  position: relative;
  @include footerSelection();
  &::after {
    content: "";
    position: absolute;
    width: 100%;
    transform: scaleX(0);
    height: 2px;
    bottom: 0;
    left: 0;
    background-color: $linkColor;
    transform-origin: bottom right;
    transition: transform 0.25s ease-out;
    border-radius: 100px;
  }
  &:hover::after {
    transform: scaleX(1);
    transform-origin: bottom left;
  }
  &:hover {
    text-shadow: 0px 0px 20px $linkColor;
  }
}
</style>
