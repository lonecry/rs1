<template>
    <div>
        <div class = "ipts">
            <span class = "ititle">状态</span>
            <span :class = "[{  gray  : detail.state=='0' },{  yellow  : detail.state=='1' },{  green  : detail.state=='2' },{  red  : detail.state=='3' }, 'ript']">{{detail.state==0?"待处理":(detail.state==1?"维修中":(detail.state==2?"已完成":"已中止"))}}</span>
        </div>
        <div class = "ipts">
            <span class = "ititle">维修班组</span>
            <span class = "ript">{{detail.banzu}}</span>
        </div>
        <div class = "ipts">
            <span class = "ititle">工长</span>
            <span class = "ript">{{detail.gongzhang}}</span>
        </div>
        <div class = "ipts">
            <span class = "ititle">工长监督电话</span>
            <span class = "ript phone" @click = 'makeacall' :data-cell = "detail.gzcell">{{detail.gzcell}}</span>
        </div>
        <div class = "ipts">
            <span class = "ititle">维修工</span>
            <span class = "ript">{{detail.weixiugong}}</span>
        </div>
        <div class = "ipts">
            <span class = "ititle">维修工电话</span>
            <span class = "ript phone" @click = 'makeacall' :data-cell = "detail.wxgcell">{{detail.wxgcell}}</span>
        </div>
        <div class = "ipts">
            <span class = "ititle">调度室举报电话</span>
            <span class = "ript phone" @click = 'makeacall' :data-cell = "detail.jubao">{{detail.jubao}}</span>
        </div>
    </div>
</template>
<script>
    export default {
        data(){
            return {
                detail: {
                    state: 1,
                    banzu: '一工队',
                    gongzhang: '张三',
                    gzcell: '13858585654',
                    weixiugong: '李四',
                    wxgcell: '13865656545',
                    jubao: '10086',
                }
            }
        },
        methods: {
            makeacall(e){
                let wx = mpvue
                let number = e.mp.currentTarget.dataset.cell
                console.log(number)
                wx.makePhoneCall({
                    phoneNumber: number //仅为示例，并非真实的电话号码
                })
            }
        },
        created(){
        },
        onShow: function (){
        },
    }
</script>
<style>
    .gray {
        color : gray;
    }
    .yellow {
        color : #ff8c2e;
    }
    .green {
        color : green;
    }
    .red {
        color : #ed283c;
    }
    .ipts {
        width         : 740rpx;
        height        : 70rpx;
        line-height   : 70rpx;
        font-size     : 28rpx;
        display       : block;
        margin        : 0 auto;
        color         : #3f4147;
        border-bottom : 1rpx solid rgba(222, 222, 222, 0.67);
    }
    .ititle {
        display      : block;
        float        : left;
        padding-left : 10rpx;
    }
    .ript {
        display       : block;
        height        : 70rpx;
        line-height   : 70rpx;
        float         : right;
        text-align    : right;
        padding-right : 10rpx;
        /*background: #ededed;*/
        width         : 525rpx;

    }
    .phone {
        color : #2d8cf0;
    }
    .active .gou {
        display : inline;
    }

</style>
