<template>
  <div id="app" class="container" v-infinite-scroll="muchMore" infinite-scroll-disabled="busy" infinite-scroll-distance="10">
    <photo 
      v-for="doge in doges"
      :item="doge"
      :key="doge"      
    />
  </div>
</template>

<script>
import Photo from './components/Photo.vue'
import axios from 'axios'
import infiniteScroll from 'vue-infinite-scroll'


export default {
  name: 'app',
  data() {
    return {
      doges: ["a5da7fd862dd4288693218f35c801e09097e7d9a","07db6458677ef1d7692811edcc272d30a58cda80","8443e858389b2e9655461406b37fa613343f7cb8","f90d45c3d5daf75e21f52d07995618055ce4effb","7b8753ee80aa95b748539d17ab532ae8ff055384","7cec6e443a4587d9df58693104632d4ba6aa81a8","b5cd5a2d04362c4d71357a58c5aa2f116cd3443e","70648ede8cd9ec01937b4b636565210d3a4f08aa","6406129367a38950e09b17a6d50a3d15f7f9e7fc","4551ba6a73f8aa382d831a23399785c7703e9923","773317b186851a8a126ce441738716d85c82ded2","445acd4dfb841747b801082b5b638302ba313a12","e4f5bb33f62fc50803dc35e986c17e97a5343ef2","966200c617bedfd9d34b5f8a302e4217da68f994","e316eb8fae279ed02cef630374206c56bd29e7db","c355e3a538076f0e7065fe0b0117b758b1fbebd5","4e7f14117a80e4a4cf052d27f8e6ce0879ab5f3f","7c0709329bd0276b9d3a351fbf9955e42a0e04b1","5cc8079401f6350301c7e23bba1bf105cbc3076f","c67b7c1277926429564a6a0f5f055d1fecb096bc","138b983e60fa3d3b8a79e4a256f695c6e148f692","6b1259488cd2cf6cceaa2b3181cc62fcb8b647c7","1286bc4da9827756db0aa51c52b3d6f56dc76c5f","fefa01faf23711a23164d426635aa48bb31a1cfb","b9ecf3c38ca4e7b1165d1629f02b31a2b398c615","0f866176eb318766c8dc0845e656ab0bceee6ad7","b4c1031dd8a7949c25ab91cb2c1a85c89e350754","debdf637d9e23ae21da828125776dfb3c5e318b2","7a9997f817ccdabac11d1f51fac563242658d654","be9f4a9182262a5ddcfe9fc4442dd1d8fb60b693"],
      meGusta: [],
      busy: false
    }
  },
  methods: {
    muchMore(){
      let vm = this
      vm.busy = true;
      // console.log('wow');
      axios.get('https://cors-anywhere.herokuapp.com/http://shibe.online/api/shibes?count=18&urls')
           .then(function (response) {
             vm.doges.push(response.data)
             vm.busy = false;
           })
    }
  },
  mounted() {

  },
  components: {
    Photo
  },
  directives: {
    infiniteScroll
  }
}
</script>

<style>
.container {
  max-width: 1024px;
  margin: 0 auto;
  display: flex;
  flex-wrap: wrap;
  position: relative;
}
.load-trigger {
  position: absolute;
  bottom: 0;
  height: 50vh;
  width: 100%;
  /* visibility: hidden; */
  background-color: aquamarine;
}
</style>
