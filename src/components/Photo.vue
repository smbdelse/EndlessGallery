<template>
    <div class="img-holder">
        <img :src="'https://cdn.shibe.online/shibes/' + item + '.jpg'" alt="">
    </div>
</template>

<script>
export default {
    props: ['item', 'value']

}
</script>

<style>
    .img-holder {
        flex-grow:1;
        padding: .2rem;
    } 

    .img-holder img {
        display: block;
        min-width: 100%;
        height: 16rem;
        object-fit: cover; 
    }
</style>
