<template>
  <div class="von-sidebar-wrapper" :class="{'left': position == 'left', 'right': position == 'right'}">
    <div class="click-block click-block-sidebar"
         :class="{'click-block-hide': !opened}"
         @click="close()"
    ></div>
    <scroll width="260" class="von-sidebar" :class="{'open': opened}">
      <slot></slot>
    </scroll>
  </div>
</template>
<style lang='scss'>
  $sidebars-z-index-default: 1;
  $sidebars-z-index-active: 11;

  [von-sidebars] {
    position: absolute;
    left: 0;
    top: 0;
    bottom: 0;
    right: 0;
    width: 100%;
    height: 100%;
    overflow: hidden;

    z-index: $sidebars-z-index-default;
    display: none;
    &.active {
      z-index: $sidebars-z-index-active;
      display: block;
    }

    &.fixed {
      position: fixed;
    }
  }

  .von-sidebar-wrapper {
    height: 100%;

    .click-block-sidebar {
      z-index: 10;
    }

    &.left .von-sidebar {
      left: 0;
      -webkit-transform: translate(-280px, 0);
      transform: translate(-280px, 0);
      margin-right: 20px;
    }

    &.right .von-sidebar {
      right: 0 !important;
      -webkit-transform: translate(280px, 0);
      transform: translate(280px, 0);
      margin-left: 20px;
    }

    // overwrite scroller style
    &.right ._v-container {
      left: auto;
    }

    .von-sidebar {
      position: fixed;
      background-color: #f5f5f5;
      height: 100%;
      top: 0;
      padding: 10px;
      z-index: 10;
      transition: transform 0.4s cubic-bezier(0.4, 0, 0, 1);
      -webkit-transition: -webkit-transform 0.4s cubic-bezier(0.4, 0, 0, 1);

      box-shadow: 0 0 10px rgba(0, 0, 0, 0.2);
      box-sizing: border-box;

      width: 260px;
      overflow-x: hidden;
      overflow-y: auto;
      -webkit-overflow-scrolling: touch;

      &.open {
        -webkit-transform: translate(0, 0);
        transform: translate(0, 0);
      }
    }
  }
</style>
<script>
  import Vue from 'vue'
  import Scroller from 'vue-scroller'

  let wrapper
  const transitionDuration = 400

  export default {
    components: {
      'scroll': Scroller
    },

    props: {
      position: {
        type: String,
        default: 'left'
      }
    },

    data() {
      return {
        opened: false
      }
    },

    ready() {
      wrapper = document.querySelector('[von-sidebars]')
    },

    methods: {
      open() {
        wrapper.classList.add('active')
        setTimeout(() => {
          this.opened = true

          Vue.nextTick(() => {
            wrapper.classList.add('fixed')
          })
        })
      },

      close() {
        wrapper.classList.remove('fixed')

        this.opened = false
        setTimeout(() => {
          wrapper.classList.remove('active')
        }, transitionDuration)
      },

      toggle() {
        if (this.opened) {
          this.close()
        } else {
          this.open()
        }
      }
    }
  }
</script>
