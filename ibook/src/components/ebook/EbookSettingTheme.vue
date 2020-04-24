<template>
    <transition name="slide-up">
        <div class="setting-wrapper" v-show="menuVisible && settingVisible === 1">
            <div class="setting-theme">
                <div class="setting-theme-item" v-for="(item, index) in themeList" :key="index" @click="setTheme(index)">
                    <div class="preview" :style="{background: item.style.body.background}" :class="{'no-border': item.style.body.background !== '#fff'}"></div>
                    <div class="text" :class="{'selected': index === defaultTheme}">{{item.name}}</div>
                </div>
            </div>
        </div>
    </transition>
</template>

<script>
    import {ebookMixin} from "../../utils/mixin";
    import {themeList} from "../../utils/book";

    export default {
        name: "EbookSettingTheme",
        mixins:[ebookMixin],
        computed:{
            themeList(){
              return themeList(this)
            }
        }
    }
</script>

<style scoped>

</style>
