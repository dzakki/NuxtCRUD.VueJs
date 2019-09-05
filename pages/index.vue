<template>
  <div class="container">
    <h1>JSON API Placheholder</h1>
    <div class="subtitle">Nama nama negara</div>
    <div class="columns">
      <div class="column">
        <div class="field is-grouped">
          <p class="control is-expanded">
            <input class="input" type="text" placeholder="Find a repository" v-model="name">
          </p>
          <p class="control">
            <button class="button is-primary" @click="add(name)" v-bind:class="{hidden: !isHidden}"> Submit </button>
            <button class="button is-warning" @click="update()" v-bind:class="{hidden: isHidden}"> Updade </button>
          </p>
        </div>
      </div>
      <div class="column">
        <ul class="panel">
          <li class="panel-heading">Daftar negara</li>
          <li class="panel-block" v-for="country in countries" :key="country.id">
            {{ country.nama }} /
            <button class="button is-info" @click=get(country.id)>Update</button> / 
            <button class="button is-danger" @click="remove(country.id)">Delete</button>
          </li>
        </ul>
      </div>
  </div>
  </div>
</template>

<script>
import {mapState, mapActions} from 'vuex'

export default {
  data: function(){
    return {
      name: '',
      isHidden: true,
      activeItem : null
    }
  },
  computed: mapState(['countries']),
  methods: {
  	add(name){
  		this.$store.dispatch('submitData', name)
  		this.name = ''
  	},
    remove(id){
      this.$store.dispatch('removeData', id)
    },
    get(id){
      this.$store.dispatch('getData', id).then((res) => {
        this.name = res.nama
        this.isHidden = false
        this.activeItem = res
      })
    },
    update(){
      this.$store.dispatch('updateData', {id: this.activeItem.id, name: this.name})
      this.isHidden = true
      this.name = ''
      this.activeItem = ''
    }
  },
}
</script>

<style media="screen">
  .hidden{
    display: none;
  }
</style>