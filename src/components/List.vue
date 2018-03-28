<template>
    <section id="items">
        <div class="container">
            <ul class="masonry">
                <li v-for="item in items" :key="item.id">
                <img :src="item.picture" alt="">
                <h3 class="title">{{item.title}}</h3>
                <span class="fa fa-map-marker"></span> {{item.locale}}
                <p>
                    {{item.description}}
                </p>
                <div class="stars">
                    <span class="fa fa-star active" v-for="star in item.stars"></span>
                    <span class="fa-stack fa-lg">
                    <i class="fa fa-circle fa-stack-2x"></i>
                    <i class="fa fa-facebook fa-stack-1x fa-inverse"></i>
                    </span>
                </div>
                </li>
            </ul>
        </div>
    </section>
</template>

<script>
export default {
    name: 'list',
    props: [
        'items'
    ]

}
</script>

<style lang="scss">

</style>
