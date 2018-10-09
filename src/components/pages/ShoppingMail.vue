<template>
    <div>
        <div class="search-bar">
            <van-row>
                <van-col span="3">
                    <img :src="locationIcon"  width="50%" class="location-icon"> 
                </van-col>
                <van-col span="16">
                    <input type="text" class="search-input">
                </van-col>
                <van-col span="5">
                    <van-button size="mini" style="margin-left: 0.6rem;">
                        查找 
                    </van-button>
                </van-col>
            </van-row>
        </div>
        <!-- swiper area -->
        <div class="swipe-area">
            <!-- 1秒钟自动切换，注意这里是:绑定了常量，也是管用的 -->
            <van-swipe :autoplay="2000">
                <!-- vue要求循环加上key,不然有可能会出错 -->
                 <van-swipe-item v-for="(banner,index) in bannerPicArray" :key="index">
                     <!-- 注意v-lazy前面没有:号 -->
                     <img v-lazy="banner.image" width="100%">
                 </van-swipe-item>
            </van-swipe>
        </div>
        <!-- type bar  -->
        <div class="type-bar">
            <div v-for="(cate,index) in category" :key="index">
                <img v-lazy="cate.image" width="90%">
                <span>{{cate.mallCategoryName}}</span>
            </div>
        </div>
        <!-- adbanner area -->
        <div>
            <img v-lazy="adBanner" width="100%">
        </div>
    </div>
</template>

<script>
    import axios from 'axios'
    export default {
         data() {
             return {
                 msg: 'Shopping Mail',
                 locationIcon: require('../../assets/images/location.png'),
                 bannerPicArray:[],
                 category:[],
                 adBanner:'',
             }
         },
         created(){
             axios({
                 url: 'https://www.easy-mock.com/mock/5bbc2c79d5a0772f6460a883/SmileVue/index',
                 method: 'get',
             }).then(response=>{
                 console.log(response)
                 if(response.status == 200){
                     this.category = response.data.data.category;
                     this.adBanner = response.data.data.advertesPicture.PICTURE_ADDRESS;
                     this.bannerPicArray = response.data.data.slides;
                 }
             })
             .catch(error=>{
                 console.log(error)
             })
            //  .then 就是意味着请求成功了
         }
    }
</script> 
 
<style scoped>
    .location-icon{ 
        padding-top: .5rem;
        padding-left: .5rem;
    }
    .search-bar{
        /* 相当于30px */
        height: 2.2rem; 
        background-color: #e5017d;
        /* 同样设置为30px，让文字在上下中居中一下 */
        line-height: 2.2rem;
        overflow: hidden;
    }
    .search-input{
        /* 设置为100%宽度是为了铺满这个input区域 */
        width: 100%;
        height: 1.3rem;
        /* 删除上面默认的边框 */
        border-top: 0px;
        border-left: 0px;
        border-right: 0px;
        border-bottom: 1px solid #fff !important;
        background-color: #e5017d; 
        /* 里面字体的颜色 */
        color: #fff;
    }
    .swipe-area{
        clear:both;
        max-height: 10rem;
        overflow: hidden;
    }  
    .type-bar{
        background-color: #fff;
        margin: 0 .3rem .3rem .3rem;
        border-radius: .3rem;
        /* 14px在任何机子下都可以看清？？ */
        font-size: 14px;
        display: flex;
        /* 横向排列 */
        flex-direction: row;
        /* 不换行，所有元素都在一行 */
        flex-wrap: nowrap;
    }
    .type-bar div{
        padding: .3rem;
        font-size: 12px;
    }
</style>