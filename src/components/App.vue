<template>
  <div id="wrapper" :class="{ 'dark-mode': isDarkMode }">
    <!-- header -->
    <div id="header">
      <a id="yc" href="http://www.ycombinator.com" target="_blank">
        <div class="yc-logo">Y</div>
      </a>
      <h1><a href="#/">Hacker News</a></h1>
      <span class="source">
        Built with <a href="http://vuejs.org" target="_blank">Vue.js</a> |
        <a href="https://github.com/Akhilesh-max/vuejs-yc-news-clone" target="_blank">Source</a>
      </span>
      <button class="theme-toggle" @click="toggleDarkMode">
        {{ isDarkMode ? '☀️' : '🌙' }}
      </button>
    </div>
    <!-- main view -->
    <router-view
      class="view"
      keep-alive
      transition
      transition-mode="out-in">
    </router-view>
  </div>
</template>

<script>
export default {
  data() {
    return {
      isDarkMode: false
    }
  },
  methods: {
    toggleDarkMode() {
      this.isDarkMode = !this.isDarkMode;
      localStorage.setItem('dark-mode', this.isDarkMode);
    }
  },
  ready() {
    if (localStorage.getItem('dark-mode') === 'true') {
      this.isDarkMode = true;
    }
  }
}
</script>

<style lang="stylus">
@import "../variables.styl"

html, body
  font-family 'Roboto', Verdana, Arial, sans-serif
  font-size 14px
  height 100%
  margin 0
  padding 0
  color #333
  background-color #f6f6ef

ul
  list-style-type none
  padding 0
  margin 0

a
  color #000
  cursor pointer
  text-decoration none
  transition color 0.2s ease
  &:hover
    color #ff6600
  
#wrapper
  background-color $bg
  position relative
  width 90%
  max-width 1200px
  min-height 80px
  margin 0 auto
  margin-top 15px
  box-shadow 0 1px 3px rgba(0,0,0,0.1)
  border-radius 2px
  transition all 0.3s ease

#wrapper.dark-mode
  background-color #1a1a1a
  color #eee
  box-shadow 0 1px 3px rgba(255,255,255,0.1)
  
  a
    color #eee
    &:hover
      color #ff6600
  
  .view
    background-color #1a1a1a
    
  .item .domain, .item .subtext
    color #999
    a 
      color #999

#header
  background-color #ff6600
  height 36px
  position relative
  border-radius 2px 2px 0 0
  display flex
  align-items center
  padding 0 8px
  h1
    font-weight bold
    font-size 16px
    display inline-block
    vertical-align middle
    margin 0
    a
      color #fff
      &:hover
        color #fff
        text-decoration underline
  .source
    color #fff
    font-size 12px
    position absolute
    top 10px
    right 48px
    a
      color #fff
      &:hover
        text-decoration underline

.theme-toggle
  position absolute
  right 12px
  top 8px
  background transparent
  border none
  color #fff
  font-size 16px
  cursor pointer
  outline none
  
#yc
  border 1px solid #fff
  margin 0 8px 0 0
  display inline-block
  vertical-align middle
  background-color #fff
  border-radius 3px
  padding 0

.yc-logo
  width 18px
  height 18px
  background-color #ff6600
  color white
  font-weight bold
  font-size 14px
  text-align center
  line-height 18px
  display block

.view
  position absolute
  background-color $bg
  width 100%
  transition all .3s ease
  box-sizing border-box
  padding 12px 20px
  &.v-enter, &.v-leave
    opacity 0
    transform translateY(5px)

@media screen and (max-width: 768px)
  html, body
    margin 0
  #wrapper
    width 100%
    margin-top 0
    border-radius 0
  #header
    border-radius 0
  .source
    display none
</style>
