<template>
    <div>
        <div class="fixed pin-t pin-l w-1/4 max-w-full text-center p-4 z-50"><button class="button__drawer" @click="open=!open" v-html="button"></button></div>         
        <div :class="{'opened':open}" class="drawer md:w-1/4 w-full">
            <transition-group name="list" tag="div">
                <photo 
                    v-for="doge in faved"
                    :item="doge"
                    :key="doge"
                    faved="all"
                    @like="$emit('unlike', doge)"
                    style="overflow:hidden"
                /> 
            </transition-group>
        </div>              
    </div>


</template>

<script>
import Photo from './Photo.vue'

export default {
  props: ['faved'],
  data(){
      return {
          open: false
      }
  },
  computed:{
      button(){
          return this.open ? '×' : '&#9829;'
      }
  },
  components: {
    Photo
  },
}
</script>

<style>
    .drawer {
        @apply fixed h-full max-w-full pin-t pin-l pb-1 pt-20 px-2 shadow-lg bg-white z-40 overflow-y-scroll overflow-x-hidden;
    }

    .button__drawer {
        @apply text-white w-12 h-12 text-3xl rounded-full shadow-lg pt-1;

        background-color: grey;
        cursor: pointer;
        transition-duration: .3s;
        outline-color: transparent;
        z-index: 120;
    }
    .button__drawer:hover {
        transform: scale(1.2);
    }
    .button__drawer:focus {
        outline-color: transparent;
    }

    .drawer {
        transition: transform .4s;
        transform: translateX(-100%);
    }
    .drawer.opened {
        transform: translateX(0);
    }

    .list-enter-active, .list-leave-active {
        transition: all .4s;
    }
    .list-enter, .list-leave-to {
        opacity: 0;
        transform: translateY(-30px);
        margin-bottom: -16rem;
    }
</style>
