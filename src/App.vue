<template>
  <div id="app" class="container" :class="{'loading': busy}" v-infinite-scroll="muchMore" infinite-scroll-disabled="busy" infinite-scroll-distance="100">
    <drawer @unlike="fav" :faved="meGusta"/>

    <photo 
      v-for="doge in doges"
      :item="doge"
      :key="doge"
      :faved="meGusta"
      @initialised="photosLoading++"
      @ready="photosLoading--"
      @like="fav(doge)"
    />
    
    <div v-show="busy" class="loader"></div>
  </div>
</template>

<script>
import Photo from './components/Photo.vue'
import Drawer from './components/Drawer.vue'
import axios from 'axios'
import infiniteScroll from 'vue-infinite-scroll'

export default {
  name: 'app',
  data() {
    return {
      doges: [],
      meGusta: [],
      busy: false,
      photosLoading: 0
    }
  },
  methods: {
    muchMore(){
      let vm = this
      vm.busy = true;
      axios.get('https://cors-anywhere.herokuapp.com/http://shibe.online/api/shibes?count=18&urls', {headers: {'Access-Control-Allow-Origin': '*'}})
           .then(function (response) {
             let unique = response.data.filter((el)=>{
               if (vm.doges.indexOf(el) > -1){
                 return false
               }
               return true
             })
             vm.doges = vm.doges.concat(unique)         
           })
    },
    fav(doge){
      let index = this.meGusta.indexOf(doge);
      if ( index > -1 ){
        this.meGusta.splice(index, 1);
      }
      else {
        this.meGusta.push(doge)

      }
    }
  },
  watch:{
    photosLoading(n){
      if(n == 0) {
        this.busy = false
      }
    },
    meGusta: {
      handler() {
        localStorage.setItem('doges', JSON.stringify(this.meGusta));
      },
      deep: true,
    }
  },
  mounted() {
    if (localStorage.getItem('doges')) this.meGusta = JSON.parse(localStorage.getItem('doges'));
  },
  components: {
    Photo, Drawer
  },
  directives: {
    infiniteScroll
  }
}
</script>

<style>
@import 'assets/styles/tailwind.postcss';

*, *::after, *::before {
  box-sizing: border-box;
}

.container {
  max-width: 1024px;
  @apply my-0 mx-auto flex flex-wrap relative pt-12;
}
.loader,
.loader::after {
  content: '';
  position: fixed;
  display: block;
  z-index: 10;
  width: calc(2rem + 2px);
  height: calc(2rem + 2px);
  border-radius: 9999px;
  border: 8px solid rgba(0, 0, 0, 0.525);
  bottom: calc(1rem - 1px);
  left: calc(50% - (1rem + 1px));  
}
.loader::after{
  width: 2rem;
  height: 2rem;
  border-radius: 9999px;
  border: 5px solid transparent;
  border-left-color:  #ffffff;  
  bottom: 1rem;
  left: calc(50% - 1rem);
  animation: spinning 2s linear infinite;
}


@keyframes spinning {
  from {
      transform: rotate(0deg)
  }
  to {
      transform: rotate(360deg)
  }
}
</style>
