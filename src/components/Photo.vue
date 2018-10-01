<template>
    <div v-show="!loading" :class="{'img-holder--loaded':!loading}" class="img-holder" v-images-loaded="shoutReady">
        <img :src="'https://cdn.shibe.online/shibes/' + item + '.jpg'" alt="">
        <button @click="likeState()" :class="{'liked': isFavourite}" class="button__like">❤</button><!-- could be a checkbox  -->
        <div class="img-holder__loader">{{getRandomWow()}}</div>
    </div>
</template>

<script>
import imagesLoaded from 'vue-images-loaded'

export default {
    props: ['item', 'faved'],
    data(){
        return {
            loading: true,
            wows: {
                adverbs: ['very', 'so', 'such', 'so much', 'much'],
                nouns: ['picture', 'doge', 'photo', 'cute']
            }
        }
    },
    mounted(){
        this.$emit('initialised');
    },
    methods:{
        shoutReady(){
            this.loading = false
            this.$emit('ready')
        },
        getRandomWow(){          
            if(Math.random() < 0.4){
                return 'wow!'
            } else {
                let randomWow = this.wows.adverbs[Math.floor(Math.random() * this.wows.adverbs.length)] +
                                ' ' +
                                this.wows.nouns[Math.floor(Math.random() * this.wows.nouns.length)] + '!'

                return randomWow
            }
        },
        likeState(){
            this.$emit('like')
        }
    },
    computed:{
      isFavourite(){
        if (this.faved=='all'){
            return false
        } else {
            return (this.faved.indexOf(this.item) > -1)
        }
      }
    },
    directives: {
        imagesLoaded
    }
}
</script>

<style>
    .img-holder {
        @apply p-1 flex-grow relative;
    }
    .img-holder__loader {
        background-image: url(../assets/img/boom.svg);
        left: calc(50% - 8rem);
        font-family: 'Comic Sans MS', sans-serif;
        color: #f32dc8;
        line-height: 16rem;

        @apply bg-no-repeat bg-center absolute w-64 h-64 pin-t text-center;

        animation: wow 1.6s cubic-bezier(.52,1.68,.82,1.54) both;
    }


    .img-holder img {
        @apply block min-w-full h-64;
        display: block;
        min-width: 100%;
        height: 16rem;
        object-fit: cover;
        animation: photo .3s ease-in 1s both; 
    }

    .button__like {
        @apply absolute pin-t pin-r m-2 text-white w-8 h-8 text-2xl rounded-full;

        background-color: rgba(255, 255, 255, 0.274);

        border: 0px;
        cursor: pointer;
        transition-duration: .3s;
        outline-color: transparent;
    }
    .button__like:hover {
        transform: scale(1.2);
    }
    .button__like:focus {
        outline-color: transparent;
    }

    .button__like.liked {
        color: rgb(255, 53, 53);
    }

    @keyframes wow {
        0%, 100% {
            transform: scale(0) rotate(-6deg);
            opacity: 0;
        }
        60% {
            transform: scale(1.2) rotate(-6deg);
            opacity: 1;
        }
    }
    @keyframes photo {
        0% {
            opacity: 0;
        }
        100% {
            opacity: 1;
        }
    }
</style>
