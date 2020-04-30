<template>
  <div class="home">
    <div>
      <button @click="decreaseNumber">Previous</button>
      <input type="text" v-model="letter" minlength="2" maxlength="2" style="width: 30px;">
      <input type="number" v-model="number" id="quantity" name="quantity" min="0" max="9999">
      <button @click="increaseNumber">Next</button>
    </div>
    <div style="display: flex; justify-content: center; align-items: center; margin-top: 10px;">
      <img src="../assets/clipboard.svg" @click="copyToClipboard(letter+number)" style="margin-right: 5px; cursor: pointer;
      "/>
      <button @click="getRandom">Random</button>
    </div>
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
    decreaseNumber() {
      this.number = parseInt(this.number) - 1
    },
    getPicture() {
      let numberText = String(this.number).padStart(4, '0')
      let url = `https://prnt.sc/${this.letter}${numberText}`

      axios.get(`https://prnt-api.herokuapp.com/?url=${url}`)
        .then(res => {
          this.link = res.data
        })
    },
    getRandom() {
      this.number = Math.floor(Math.random() * 10000) 
    },
    doSomething() {
      this.increaseNumber()
      this.getPicture()
    },
    copyToClipboard(str) {
      const el = document.createElement('textarea');
      el.value = str;
      document.body.appendChild(el);
      el.select();
      document.execCommand('copy');
      document.body.removeChild(el);
    }
  }
}
</script>
