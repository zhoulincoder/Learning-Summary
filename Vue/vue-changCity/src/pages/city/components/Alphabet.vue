<template>
  <ul class="list">
    <li class="item"
      :class="{'activeItem': item === currentLetter }"
      v-for="item of letters"
      :key="item"
      :ref="item"
      @click="handleLetterClick"
      @touchstart.prevent="handleTouchStart"
      @touchmove.prevent="handleTouchMove"
      @touchend.prevent="handleTouchEnd"
      >{{item}}</li>
  </ul>
</template>

<script>
export default {
  name: 'CityAlphabet',
  props: {
    cities: Object,
    letter: String
  },
  data () {
    return {
      touchStatus: false,
      currentLetter: 'A',
      timer: null,
    }
  },
  updated () {
    this.startY = this.$refs['A'][0].offsetTop
  },
  computed: {
    
    letters () {
      const letters = []
      for (let i in this.cities) {
        letters.push(i)
      }
      return letters
    },
    
  },
  watch: {
    letter () {
      if(this.timer) {
        clearTimeout(this.timer)
      }
      this.timer = setTimeout(() => this.currentLetter = this.letter, 16)
      
    }
  },
  methods: {
    handleLetterClick (e) {
      this.currentLetter = e.target.innerText
      this.$emit('change', this.currentLetter)
    },
    handleTouchStart () {
      this.touchStatus = true
    },
    handleTouchMove (e) {
      // 增加节流
      if (this.touchStatus) {
        if (this.timer) {
          clearTimeout(this.timer)
        }
        this.timer = setTimeout(() => {
          const touchY = e.touches[0].clientY - 80 // e.touches[0].clientY 触摸位置高度
          const index = Math.floor((touchY - this.startY) / 20)          
          if (index >= 0 && index < this.letters.length) {
            this.currentLetter = this.letters[index]
            this.$emit('change', this.currentLetter)
          }
        }, 16)
      }
    },
    handleTouchEnd () {
      this.touchStatus = false
    }
  }
}
</script>

<style lang="stylus" scoped>
  .list
    display flex
    flex-direction column
    justify-content center
    position absolute
    right 4px
    top 80px
    bottom 0
    width 20px
    .item
      text-align center
      line-height 20px
      color #C62F2F
    .activeItem
      background #ddd
      border-radius 10px
</style>
