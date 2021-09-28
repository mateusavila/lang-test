<script>
  const prepare = cpf => cpf.trim().split('').filter(i => !['.', '-', ' '].includes(i)).map(Number)
  console.log(prepare('047.438.449-75'))
  export default {
    data () {
      return {
        list: [],
        lang: ''
      }
    },
    mounted () {
      this.all()
    },
    methods: {
      async all () {
        const request = await fetch('https://restcountries.com/v3/all', {mode: 'cors'})
        this.list = await request.json() 
      },
      async filterLang (lang) {
        const request = await fetch(`https://restcountries.com/v3/lang/${lang}`, {mode: 'cors'})
        this.list = await request.json() 
        this.lang = lang
      },
      getLang (lang) {
        const obj = JSON.parse(JSON.stringify(lang))
        const keys = Object.keys(obj)
        const values = Object.values(obj)
        const arr = []
        for (var i = 0; i < keys.length; i++) {
          arr.push({
            title: values[i],
            lang: keys[i]
          })
        }
        return arr
      }
    }
  }
</script>

<template>
  <div class="content">
    <h1>Lista de países <a href="#voltar" @click.prevent="all" v-if="lang !== ''">Voltar</a></h1>
    <div class="list">
      <div class="item" v-for="(item, index) in list" :key="index">
        <div class="item-flag">
          <img :src="item.flags[0]" :alt="item.name.common" width="80" height="60" loading="lazy">
          <p>{{item.name.common}} <br><small v-if="item.languages"><a v-for="(lang, num) in getLang(item.languages)" :key="num" @click.prevent="filterLang(lang.lang)" :href="'#'+lang.lang">{{lang.title}}</a></small></p>
        </div>
      </div>
    </div>
  </div>  
</template>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
.list {
  display: grid;
  grid-template-columns: 1fr 1fr 1fr 1fr 1fr 1fr;
}
.list small a {
  margin: 0 5px;
  display: inline-block;
}
</style>
