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
          return this.open ? '×' : '<svg style="width:1em" height="1em" viewBox="0 0 51.997 51.997"><path fill="currentColor" d="M51.911,16.242C51.152,7.888,45.239,1.827,37.839,1.827c-4.93,0-9.444,2.653-11.984,6.905  c-2.517-4.307-6.846-6.906-11.697-6.906c-7.399,0-13.313,6.061-14.071,14.415c-0.06,0.369-0.306,2.311,0.442,5.478  c1.078,4.568,3.568,8.723,7.199,12.013l18.115,16.439l18.426-16.438c3.631-3.291,6.121-7.445,7.199-12.014  C52.216,18.553,51.97,16.611,51.911,16.242z"></path></svg>'
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
