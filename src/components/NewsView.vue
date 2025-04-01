<template>
  <div class="news-view" :class="{ loading: !items.length }">
    <!-- item list -->
    <item
      v-for="item in items"
      :item="item"
      :index="$index | formatItemIndex"
      track-by="id">
    </item>
    <!-- navigation -->
    <div class="nav" v-show="items.length > 0">
      <a v-if="page > 1" :href="'#/news/' + (page - 1)">&lt; prev</a>
      <a v-if="page < 4" :href="'#/news/' + (page + 1)">more...</a>
    </div>
  </div>
</template>

<script>
import store from '../store'
import Item from './Item.vue'

export default {

  name: 'NewsView',

  components: {
    Item
  },

  data () {
    return {
      page: 1,
      items: []
    }
  },

  route: {
    data ({ to }) {
      // This is the route data hook. It gets called every time the route
      // changes while this component is active.
      // 
      // What we are doing:
      // 
      // 1. Get the `to` route using ES2015 argument destructuring;
      // 2. Get the `page` param and cast it to a Number;
      // 3. Fetch the items from the store, which returns a Promise containing
      //    the fetched items;
      // 4. Chain the Promise and return the final data for the component.
      //    Note we are waiting until the items are resolved before resolving
      //    the entire object, because we don't want to update the page before
      //    the items are fetched.
      const page = +to.params.page
      document.title = 'Hacker News Clone by Akhilesh-max'
      return store.fetchItemsByPage(page).then(items => ({
        page,
        items
      }))
    }
  },

  created () {
    store.on('topstories-updated', this.update)
  },

  destroyed () {
    store.removeListener('topstories-updated', this.update)
  },

  methods: {
    update () {
      store.fetchItemsByPage(this.page).then(items => {
        this.items = items
      })
    }
  },

  filters: {
    formatItemIndex (index) {
      return (this.page - 1) * store.storiesPerPage + index + 1
    }
  }
}
</script>

<style lang="stylus">
.news-view
  padding-left 5px
  padding-right 15px
  min-height 500px
  position relative
  &.loading:before
    content "Loading..."
    position absolute
    top 16px
    left 20px
    font-weight bold
    color #ff6600
    animation pulse 1.5s infinite
  .nav
    padding 10px 10px 10px 40px
    margin-top 10px
    border-top 2px solid #ff6600
    display flex
    align-items center
    a
      margin-right 15px
      padding 5px 10px
      color #ff6600
      font-weight 500
      border-radius 3px
      transition all 0.2s ease
      &:hover
        background-color rgba(255, 102, 0, 0.1)
        text-decoration none

@keyframes pulse
  0%
    opacity 0.6
  50%
    opacity 1
  100%
    opacity 0.6
</style>
