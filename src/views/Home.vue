<template>
  <div class="home">
    <input type="text" v-model="letter" minlength="2" maxlength="2" style="width: 30px;">
    <input type="number" v-model="number" id="quantity" name="quantity" min="0" max="9999">
    <button @click="doSomething">Get Next Screenshot</button>
    <div class="clear-both"></div>
    <img :src="link" width="700" style="margin-top: 50px">
  </div>
</template>

<script>
import axios from 'axios'
export default {
  name: 'Home',
  data () {
    return {
      link: 'afsdasdfs',
      letter: 'aa',
      number: 0
    }
  },
  watch: {
    number() {
      this.getPicture()
    },
    letter() {
      this.getPicture()
    }
  },
  methods: {
    increaseNumber() {
      this.number = parseInt(this.number) + 1
    },
    getPicture() {
      let numberText = String(this.number).padStart(4, '0')
      let url = `https://prnt.sc/${this.letter}${numberText}`

      axios.get(`https://prnt-api.herokuapp.com/?url=${url}`)
        .then(res => {
          this.link = res.data
        })
    },
    doSomething() {
      this.increaseNumber()
      this.getPicture()
    }
  }
}
</script>
