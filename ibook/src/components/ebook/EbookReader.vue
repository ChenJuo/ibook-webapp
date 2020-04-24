<template>
    <div class="ebook-reader">
       <div id="read">

       </div>
    </div>
</template>

<script>
    import {ebookMixin} from "../../utils/mixin";
    import {getFontFamily, saveFontFamily,getFontSize,saveFontSize} from "../../utils/localStorage";
    import Epub from 'epubjs';

    global.ePub = Epub;
    export default {
        name: "EbookReader",
        mixins:[ebookMixin],
        methods:{
            prevPage(){
                if(this.rendition){
                    this.rendition.prev();
                    this.hideTitleAndMenu();
                }
            },
            nextPage(){
                if(this.rendition){
                    this.rendition.next();
                    this.hideTitleAndMenu();
                }
            },
            toggleTitleAndMenu(){
                if(this.menuVisible){
                    this.setSettingVisible(-1);
                    this.setFontFamilyVisible(false)
                }
                this.setMenuVisible(!this.menuVisible)
            },
            hideTitleAndMenu(){
                this.setMenuVisible(false);
                this.setSettingVisible(-1);
                this.setFontFamilyVisible(false)
            },
            initFontSize(){
                //加载设置字号配置
                let fontSize = getFontSize(this.fileName);
                if(!fontSize){
                    saveFontSize(this.fileName,this.defaultFontSize)
                }else{
                    this.rendition.themes.fontSize(fontSize);
                    this.setDefaultFontSize(fontSize)
                }
            },
            initFontFontFamily(){
                //加载设置字体配置
                let font = getFontFamily(this.fileName);
                if(!font){
                    saveFontFamily(this.fileName,this.defaultFontFamily)
                }else{
                    this.rendition.themes.font(font);
                    this.setDefaultFontFamily(font)
                }
            },
            initEpub(){
                const url ='http://192.168.1.106:8889/epub/'+this.fileName+'.epub';
                this.book = new Epub(url);
                this.setCurrentBook(this.book);
                this.rendition= this.book.renderTo('read',{
                    width:innerWidth,
                    height:innerHeight,
                    method:'default',
                });
                this.rendition.display().then(() =>{
                    this.initFontSize();
                    this.initFontFontFamily()
                });
                this.rendition.on('touchstart',event => {
                    this.touchStartX = event.changedTouches[0].clientX;
                    this.touchStartTime = event.timeStamp;
                });
                this.rendition.on('touchend',event => {
                    const offsetX = event.changedTouches[0].clientX - this.touchStartX;
                    const time = event.timeStamp - this.touchStartTime;
                    if(time < 500 && offsetX > 40){
                        this.prevPage()
                    }else if(time < 500 && offsetX < -40){
                        this.nextPage()
                    }else{
                        this.toggleTitleAndMenu()
                    }
                    event.preventDefault();
                    event.stopPropagation();
                });
                this.rendition.hooks.content.register(contents => {
                    Promise.all(
                        [
                            contents.addStylesheet(`${process.env.VUE_APP_RES_URL}/fonts/cabin.css`),
                            contents.addStylesheet(`${process.env.VUE_APP_RES_URL}/fonts/daysOne.css`),
                            contents.addStylesheet(`${process.env.VUE_APP_RES_URL}/fonts/tangerine.css`)
                        ]
                    ).then(() =>{
                        console.log('……字体加载完毕')
                    })
                })
            }
        },
        mounted(){
            this.setFileName(this.$route.params.fileName.split('|').join('/')).then(() => {
                this.initEpub()
            });
        },
    }
</script>

<style lang="scss" rel="stylesheet/scss" scoped>
    @import "../../assets/styles/global";
</style>
