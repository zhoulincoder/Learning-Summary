<template>
  <div class="list" ref="wrapper">
    <div>
      <div class="area">
        <div class="title border-topbottom">当前城市</div>
        <div class="button-list">
          <div class="button-wrapper">
            <div class="button">
              南昌
            </div>
          </div>
        </div>
      </div>
      <div class="area">
        <div class="title border-topbottom">热门城市</div>
        <div class="button-list">
          <div 
            class="button-wrapper" 
            v-for="item of hotCities" 
            :key="item.id" 
            @click="handleCityClick(item.name)">
            <div class="button">
              {{item.name}}
            </div>
          </div>
        </div>
      </div>
      <div class="area" v-for="(item, key) of cities" :key="key" :ref="key">
        <div class="title border-topbottom">{{key}}</div>
        <div class="item-list">
          <div 
            class="item border-bottom" 
            v-for="innerItem of item" 
            :key="innerItem.id" 
            @click="handleCityClick(innerItem.name)"
            >{{innerItem.name}}</div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import Bscroll from 'better-scroll'
export default {
  name: 'CityList',
  props: {
    hotCities: Array,
    cities: Object,
    Lletter: String
  },
  data () {
    return {
      timer: null,
      scrollY: '',
      keySumHeights: [],
      letters: [],
    }
  },
  mounted () {
    this.$nextTick(() => {
      this._initScroll()
    })
  },
  updated () {
    this._getSumHeight()
  },
  watch: {
    Lletter () {
      if (this.Lletter) {
        const element = this.$refs[this.Lletter][0]
        this.scroll.scrollToElement(element)
      }
    },
    scrollY () {
      // console.log(this.scrollY);
      if(this.timer) {
        clearTimeout(this.timer)
      }
      this.timer = setTimeout(() => {
        let temps = this.keySumHeights
        for(let i = 0; i < temps.length-1; i++) {
          let height1 = temps[i]
          let height2 = temps[i+1]
          if(this.scrollY >= height1 && this.scrollY < height2) {
            this.$emit('change', this.letters[i])
          }
        }
      }, 16)
      
    }
  },
  methods: {
    _initScroll () {
      this.scroll = new Bscroll(this.$refs.wrapper, {
        // click: true,
        probeType: 3
      })
      this.scroll.on('scroll', (pos) => {
        this.scrollY = Math.abs(Math.ceil(pos.y))
      })
    },
    _getSumHeight () {
      let tempHeight = []
      let tempLetters = []
      let height = 0
      let tt = []
      tempHeight.push(height)
      for(let key in this.cities) {
        tempLetters.push(key)
        tt.push(this.$refs[key][0].clientHeight)
        height += this.$refs[key][0].clientHeight
        tempHeight.push(height)
      }
      console.log(tt);
      
      let res = tempHeight.map(item => {
        if(item === 0) {
          return 0
        }
        return item + 180
      })
      this.letters = tempLetters
      this.keySumHeights = res
      console.log(res); 
    },
    handleCityClick (city) {
      console.log(city);      
    }
  }
  
}
</script>

<style lang="stylus" scoped>
  .border-topbottom
    &:before
      border-color #ccc
    &:after
      border-color #ccc
  .border-bottem
    &:before
      border-color #ccc
  .list
    position absolute
    top 43px
    right 0
    left 0
    bottom 0
    overflow hidden
    .title
      line-height 28px
      background #eee
      color #666
      font-size 13px
      padding-left 10px
    .button-list
      padding 5px 30px 5px 5px
      overflow hidden
      .button-wrapper
        width 33.33%
        float left
        .button
          margin 5px
          padding 5px 0
          text-align center
          border 1px solid #ccc
          border-radius 3px
    .item-list
      .item
        line-height 38px
        padding-left 10px
</style>
