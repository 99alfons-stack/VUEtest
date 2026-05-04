<template>
  <h1>Hello VUE</h1>
  <p>välkommen {{ user.firstName}} {{ user.lastName }} ! </p>

  <div>
    Du har klickat {{ Clicked }} gånger!!
    <button @click="onClick">Klicka på mig </button>
  </div>

  <div v-if="Clicked > 5" >
    Bra klickat!
  </div>
</template>

<script>

export default {
  name: 'App',


  data(){
    return{
      user: {firstName: "Alfons", lastName: "B-son"},
      Clicked: 0,
    }
  },
  methods:{
    onClick(){
      console.log("antal klick");
      this.Clicked ++
    }
  }
}


</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
b 