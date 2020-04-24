<template>
    <div id="app">
        <router-view/>
    </div>
</template>

<script>
    /*import {mapGetters} from 'vuex'
    const getters ={
        a:() => 1,
        b:() => 2,
        c:() => 3
    };
    function fn(keys){
        const data = {};
        keys.forEach(key => {
            //Object.prototype.hasOwnProperty.call(getters, "key")判断数组中是否有该元素
            if(Object.prototype.hasOwnProperty.call(getters, "key")){
                data[key] = getters[key]
            }
        });
        return data
    }*/
    export default {
        name: 'App',
        computed: {
           /* ...mapGetters(['test']),
            ...fn(['a','b','c'])*/
        },
        mounted() {
           /* this.$store.dispatch('setTest', 10).then(() => {
                console.log(this.$store.state.book.test)
            })*/
           /*console.log(this.a,this.b)*/
        }
    }
    document.addEventListener('DOMContentLoaded', () => {
        const html = document.querySelector('html');
        let fontSize = window.innerWidth / 10;
        fontSize = fontSize > 50 ? 50 : fontSize;
        html.style.fontSize = fontSize + 'px'
    })
</script>

<style lang="scss" scoped>
    #app{
        width: 100%;
        height: 100%;
        overflow: hidden;
    }
</style>
