<template>
  <div>
    <city-header />
    <city-list :cities="cities" :hotCities="hotCities" :Lletter="Lletter" @change="listChangeLetter"/>
    <city-alphabet :cities="cities" @change="AlphaChangeLetter" :letter="letter"/>
  </div>
</template>

<script>
import axios from 'axios'
import CityHeader from './components/Header'
import CityList from './components/List'
import CityAlphabet from './components/Alphabet'
export default {
  name: 'City',
  data () {
    return {
      cities: {},
      hotCities: [],
      letter: '',
      Lletter: ''
    }
  },
  components: {
    CityHeader,
    CityList,
    CityAlphabet
  },
  mounted () {
    this.getCityInfo()
  },
  methods: {
    getCityInfo () {
      axios.get('/api/city.json')
        .then(this.handleGetCityInfoSucc)
    },
    handleGetCityInfoSucc (res) {
      res = res.data
      if (res.ret && res.data) {
        const data = res.data
        this.cities = data.cities
        this.hotCities = data.hotCities
      }
    },
    listChangeLetter (letter) {
      this.letter = letter
    },
    AlphaChangeLetter (letter) {
      this.Lletter = letter
    }
  }
}
</script>

<style lang="stylus" scoped>

</style>