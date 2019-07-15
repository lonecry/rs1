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
        <div class = "ipts">
            <span class = "ititle" @click = 'forNav'>forNav</span>
        </div>
        <div class = "detailbox">
            <span class = "xqtxt">详情:</span>
            <div class = "detail">
                <span class = "bxlist"><span class = "spans inspan">报修单号:</span><span class = "spans  ">{{detail.origin.danhao}}</span></span>
                <span class = "bxlist"><span class = "spans inspan">报修时间:</span><span class = "spans    ">{{detail.origin.time}}</span></span>
                <span class = "bxlist"><span class = "spans inspan">报修人姓名:</span><span class = "spans  ">{{detail.origin.name}}</span></span>
                <span class = "bxlist"><span class = "spans inspan">手机号:</span>  <span class = "spans  phone" @click = 'makeacall' :data-cell = "detail.origin.phone">{{detail.origin.phone}}</span></span>
                <span class = "bxlist"><span class = "spans inspan">报修类型:</span>  <span class = "spans  ">{{detail.origin.type}}</span></span>
                <span class = "bxlist">
                    <span class = "spans inspan">报修内容:</span>
                    <span class = "neirong" v-for = "(item,index) in detail.origin.content" :key = "index">{{item}}</span>
                </span>
                <span class = "bxlist">
                    <span class = "spans inspan">现场图片:</span>
                      <div>
                        <div class = "imgbox" v-for = "(item,index) in detail.origin.imgsUrl" :key = "index">
                            <img :src = "item" :mode = "'widthFix'" @click = 'preview(index)' class = "slt" alt = "缩略图">
                        </div>
                      </div>
                </span>
                <span class = "bxlist"><span class = "spans inspan">车站:</span><i-icon style="position:relative;top:-4rpx;"type = "coordinates_fill" size = "26" color = "#2d8cf0" class = "usericon"/><span class = "spans  ">{{detail.origin.station}}</span></span>
                <span class = "bxlist"><span class = "spans inspan">详细位置:</span> {{detail.origin.address}} </span>
                <span class = "bxlist"><span class = "spans inspan">台单号:</span> {{detail.origin.taidanhao}} </span>
            </div>
        </div>
        <i-toast id = "toast"/>
    </div>
</template>
<script>
    import {$Toast} from '../../../static/iview/base/index'

    export default {
        data(){
            return {
                detail: {
                    detailShow: 1,
                    detailurl: '',
                    state: 1,
                    banzu: '一工队',
                    gongzhang: '张三',
                    gzcell: '13858585654',
                    weixiugong: '李四',
                    wxgcell: '13865656545',
                    jubao: '10086',
                    location: '',
                    origin: {
                        danhao: 'WX1245151424',
                        time: "2019年7月04日 18:44",
                        name: "张三",
                        phone: "13854587485",
                        type: '水电问题',
                        content: ['度数不转', '水表度数块', '其他问题', '还有问题', '能量不打钩', '旖旎你水电'],
                        imgsUrl: ['http://www.simpleqq.com/index/imgs/tab/tab4.jpg', "https://www.simpleqq.com/index/imgs/imgdemo.jpg"],
                        station: "杭州南站",
                        address: "杭州南站习广场东侧候车厅小隔间大阳台小浴室的拐角的洞洞里",
                        taidanhao: 'this is off'
                    },
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
            },
            forNav: function (){
                // 地图选择
                // wx.chooseLocation({
                //     success: function (res){
                //         // success
                //         console.log(res, "location")
                //         console.log(res.name)
                //         console.log(res.latitude)
                //         console.log(res.longitude)
                //
                //     },
                //     fail: function (){
                //     },
                //     complete: function (){
                //     }
                // })
                // wx.vibrateShort()
                var _this = this
                _this.getPermission(_this).then(() =>{
                    var destination = {
                        latitude: _this.data.latitude,
                        longitude: _this.data.longitude,
                        name: "中国浙江省杭州市西湖区莫干山路111号",
                        address: "浙江省杭州市拱墅区米市巷街道半道红社区西南方向",
                        scale: 18
                    }
                    wx.setStorageSync('destination', destination)
                    mpvue.navigateTo({
                        url: '../locationSearch/main',
                    })
                })
            },
            getPermission: function (obj){
                var wx = mpvue
                var pmse = new Promise((resolve, reject) =>{
                    wx.getLocation({
                        type: 'gcj02', //返回可以用于wx.openLocation的经纬度
                        success: function (res){
                            console.log(res);
                            obj.location = {
                                latitude: res.latitude,     //latitude,
                                longitude: res.longitude      //latitude
                            }
                            resolve()
                        },
                        fail: function (){
                            wx.getSetting({
                                success: function (res){
                                    var statu = res.authSetting;
                                    if (!statu['scope.userLocation']) {
                                        wx.showModal({
                                            title: '是否授权当前位置',
                                            content: '需要获取您的地理位置，请确认授权，否则地图功能将无法使用',
                                            success: function (tip){
                                                if (tip.confirm) {
                                                    wx.openSetting({
                                                        success: function (data){
                                                            if (data.authSetting["scope.userLocation"] === true) {
                                                                $Toast({
                                                                    title: '授权成功',
                                                                    icon: 'success',
                                                                    duration: 1
                                                                })
                                                                //授权成功之后，再调用chooseLocation选择地方
                                                                wx.getLocation({
                                                                    type: 'gcj02', //返回可以用于wx.openLocation的经纬度
                                                                    success: function (res){
                                                                        console.log(res);
                                                                        obj.location = {
                                                                            latitude: res.latitude,     //latitude,
                                                                            longitude: res.longitude      //latitude
                                                                        }
                                                                        resolve()
                                                                    },
                                                                })
                                                            } else {
                                                                $Toast({
                                                                    title: '授权失败',
                                                                    icon: 'success',
                                                                    duration: 1
                                                                })
                                                                reject()
                                                            }
                                                        }
                                                    })
                                                }
                                            }
                                        })
                                    }
                                },
                                fail: function (res){
                                    $Toast({
                                        title: '调用授权窗口失败',
                                        icon: 'success',
                                        duration: 1
                                    })
                                    reject()
                                }
                            })
                        }
                    })
                })
                return pmse
            },
            preview: function (key){
                wx.previewImage({
                    current: this.detail.origin.imgsUrl[key], // 当前显示图片的http链接
                    urls: this.detail.origin.imgsUrl  // 需要预览的图片http链接列表
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
    .detailbox {
        width:100%;
        padding:20rpx;
        box-sizing:border-box;
        -webkit-box-sizing:border-box;
        padding-bottom:150rpx;


    }
    .xqtxt {
        font-weight : bold;
    }
    .detail {
        width:100%;
        min-height:600rpx;
        border:1rpx solid gray;
        margin-top:10rpx;
        padding:10rpx;
        box-sizing:border-box;

    }
    .bxlist {
        width        : 100%;
        display      : block;
        min-height   : 50rpx;
        line-height  : 50rpx;
        font-size    : 28rpx;
        text-align   : justify;
        padding-left : 10rpx;
        box-sizing   : border-box;
    }
    .spans, .neirong {
        display : inline-block;
    }
    .neirong {
        border        : 1rpx solid #b2b8c5;
        border-radius : 10rpx;
        margin        : 0 10rpx 10rpx 10rpx;
        box-sizing    : border-box;
        padding       : 0 10rpx;
        font-size     : 24rpx;

    }
    .inspan {
        margin-right : 20rpx;
    }
    .imgbox {
        display    : inline-block;
        width      : 200rpx;
        height     : 200rpx;
        background : rgba(170, 255, 119, 0.6);
        margin     : 0 20rpx 0 0;
        position   : relative;
        overflow   : hidden;
    }
    .slt {
        position          : absolute;
        width             : 100%;
        height            : auto;
        display           : block;
        left              : 0;
        top               : 50%;
        transform         : translateY(-50%);
        -webkit-transform : translateY(-50%);
    }
    .imgdetail {
        background : black;
        width      : 100%;
        height     : 100vh;
        position   : fixed;
        left       : 0;
        top        : 0;
    }
</style>
