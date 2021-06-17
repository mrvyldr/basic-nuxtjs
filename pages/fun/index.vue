<template>
  <div>
    <div>{{ content }}</div>
    <div>{{ counter }}</div>
    <button @click="refresh" class="button--green">Refresh</button>
    <button @click="increment()" class="button--grey">increment</button>
    <h1>I am rendered on the {{ renderedOn }} side</h1>

    <button @click="fetchData()" class="button--green">Fetch Data</button>
    <p>{{fetch}}</p>
  </div>
</template>

<script>
import { ref, reactive } from 'vue'
import Vue from 'vue'

export default {
    asyncData() {
      return { content: 'Created at: ' + new Date() , counter: 0, fetch:'',}
    },
    data() {
      return {
        renderedOn: process.client ? 'client' : 'server',
        
        //counter: 0
      }
    },
    methods: {
      refresh() {
        this.$nuxt.refresh()
      },
      increment() {
        console.log("arttır")
        return this.counter++
      },
      async fetchData() {
        const result = await this.$axios.get('http://localhost:3000/exampleapi').catch((err) => err)
        const { status } = result
        if (status === 200) {
          const { data } = result
          this.fetch = data
          console.log("data ->",{data})
          console.log("result ->",result)

          return this.fetch
        }
        
      },      
    },

}
</script>