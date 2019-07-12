<template>
    <div class = " box">
        <i-button @click = "toggleLeft" type = "ghost">左边弹出1</i-button>
        <i-tabs current = "{{ current_scroll }}" scroll bindchange = "handleChangeScroll">
            <i-tab key = "tab1" title = "选项1"></i-tab>
            <i-tab key = "tab2" title = "选项2"></i-tab>
            <i-tab key = "tab3" title = "选项3"></i-tab>
            <i-tab key = "tab4" title = "选项4"></i-tab>
            <i-tab key = "tab5" title = "选项5"></i-tab>
            <i-tab key = "tab6" title = "选项6"></i-tab>
            <i-tab key = "tab7" title = "选项7"></i-tab>
            <i-tab key = "tab8" title = "选项8"></i-tab>
            <i-tab key = "tab9" title = "选项9"></i-tab>
        </i-tabs>
        <i-drawer mode = "left" @click = "toggleLeft" :visible = "showleft" class = "i-drawer-mask">
            <view class = "demo-container">
                单击遮罩层关闭
            </view>
        </i-drawer>
    </div>
</template>
<script>
    import {$Toast} from '../../../static/iview/base/index'

    export default {
        data(){
            return {
                showleft: false,
                current_scroll: 'tab1'
            }
        },
        components: {},
        methods: {
            toggleLeft(){
                console.log(' draw true')
                this.showleft = !this.showleft
            },
            handleChangeScroll({detail}){
                this.setData({
                    current_scroll: detail.key
                });
            }
        },
        created(){
            // let app = getApp()
        }
    }
</script>
<style scoped>
    .miantitle {
        width      : 100%;
        display    : block;
        text-align : center;
        font-size  : 35rpx;
        position   : absolute;
        top        : 22rpx;
        left       : 0;

    }
    .demo-container {
        width      : 400rpx;
        height     : 100vh;
        background : white;

    }
    .i-drawer-mask {
        background : rgba(28, 36, 56, 0.29);
    }
</style>
