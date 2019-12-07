<template>
  <div>
    <router-link to="/city">
      <div class="linkTo">
        选择城市
      </div>
    </router-link>
  </div>
</template>

<script>
export default {
  name: 'HelloWorld',
  data () {
    return {
      msg: 'Welcome to Your Vue.js App'
    }
  }
}
</script>

<style lang="stylus" scoped>
  .linkTo
    width 200px
    font-size 32px
    margin 0 auto
    text-align center
    color #aaa
  
</style>
