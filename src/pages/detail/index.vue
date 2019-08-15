<template>
    <div style="padding-bottom:150rpx;">
        <div class="ipts">
            <span class="ititle">状态</span>
            <span
                :class="[{  gray  : detail.state=='0' },{  yellow  : detail.state=='1' },{  green  : detail.state=='2' },{  red  : detail.state=='3' }, 'ript']">{{detail.state==0?"待处理":(detail.state==1?"维修中":(detail.state==2?"已完成":"已中止"))}}</span>
        </div>
        <!-- <div class="ipts">
             <span class="ititle">维修班组</span>
             <span class="ript">{{detail.banzu}}</span>
         </div>-->
        <div class="ipts" v-if="(detail.state==1||detail.state==2||detail.state==3)&&(detail.gongzhang)">
            <span class="ititle">工长</span>
            <span class="ript">{{detail.gongzhang}}</span>
        </div>
        <div class="ipts" v-if="(detail.state==1||detail.state==2||detail.state==3)&&(detail.gzcell)">
            <span class="ititle">工长监督电话</span>
            <span class="ript phone" @click='makeacall' :data-cell="detail.gzcell">{{detail.gzcell}}</span>
        </div>
        <div class="ipts" v-if="(detail.state==1||detail.state==2||detail.state==3)&&(detail.weixiugong)">
            <span class="ititle">维修工</span>
            <span class="ript">{{detail.weixiugong}}</span>
        </div>
        <!--   <div class="ipts">
               <span class="ititle">维修工电话</span>
               <span class="ript phone" @click='makeacall' :data-cell="detail.wxgcell">{{detail.wxgcell}}</span>
           </div>-->
        <div class="ipts" v-if="detail.state==1||detail.state==2||detail.state==3">
            <span class="ititle">调度室举报电话</span>
            <span class="ript phone" @click='makeacall' :data-cell="detail.jubao">{{detail.jubao}}</span>
        </div>

        <!--<div class="ipts">-->
        <!--<span class="ititle" @click='forNav'>forNav</span>-->
        <!--</div>-->
        <div class="detailbox">
            <span class="xqtxt">详情:
                <i-icon type="label" size="20"/></span>
            <div class="detail">
                <span class="bxlist">
                    <span class="spans inspan">报修单号:</span>
                    <span class="spans  ">{{detail.origin.danhao}}</span>
                </span>
                <span class="bxlist">
                    <span class="spans inspan">报修时间:</span>
                    <span class="spans    ">{{detail.origin.time}}</span>
                </span>
                <!-- <span class="bxlist">
                     <span class="spans inspan">报修人姓名:</span>
                     <span class="spans  ">{{detail.origin.name}}</span>
                 </span>
                 <span class="bxlist">
                     <span class="spans inspan">手机号:</span>
                     <span class="spans  phone" @click='makeacall' :data-cell="detail.origin.phone">{{detail.origin.phone}}</span>
                 </span>-->
                <span class="bxlist"><span class="spans inspan">报修类型:</span> <span class="spans  ">{{detail.origin.type}}</span></span>
                <span class="bxlist">
                    <span class="spans inspan">报修内容:</span>
                    <span class="neirong" v-for="(item,index) in detail.origin.content" :key="index">{{item}}</span>
                </span>
                <span class="bxlist">
                    <span class="spans inspan">现场图片:</span>
                    <div>
                        <div class="imgbox" v-for="(item,index) in detail.origin.imgsUrl" :key="index">
                            <img :src="item" :mode="'widthFix'" @click='preview(index)' class="slt" alt="缩略图">
                        </div>
                    </div>
                </span>

                <span class="bxlist"><span class="spans inspan">车站:</span>    <i-icon
                    style="position:relative;top:-4rpx;" type="coordinates_fill" size="24" color="#2d8cf0"
                    class="usericon"/><span class="spans  ">{{detail.origin.station}}</span></span>
                <span class="bxlist" style="text-align: left; word-break:break-all; ">   <span class="spans inspan">详细位置:</span>  {{detail.origin.address}}  </span>

                <span class="bxlist" v-if="detail.origin.taidanhao"><span class="spans inspan">台单号:</span> {{detail.origin.taidanhao}} </span>

                <span style="width: 98%;border-top: 1px solid #a0a0a0;height: 14rpx;  display: block;"></span>
                <span class="bxlist" v-if="detail.state==2"
                      style="text-align: left; word-break:break-all;">
                    <span class="spans inspan">完成时间:</span>  {{detail.origin.EndTime}}  </span>
                <span class="bxlist" v-if="weixiuimgs.length>0" style="overflow: hidden;">

                             <span class="spans inspan" style="display: block">维修图片:</span>
                        <div class="imgbox" v-for="(item,index) in weixiuimgs" :key="index" style="float:left">
                            <img :src="item" :mode="'widthFix'" @click='wxpreview(index)' class="slt" alt="缩略图">
                        </div>

                    <!--                    <div v-else style="display: inline"> 请耐心等待维修工前去维修</div>-->
                </span>
                <span class="bxlist" v-if="ratealready&& detail.state!=='3'"><span class="spans inspan">满意度:</span> {{ratetxt}}
                <van-rate count="3" size="20" :value="ratecode"
                          style="width: 50%;display: inline-block;position: relative;top: 14rpx;"
                /></span>
            </div>
        </div>
        <div class="ipts" v-if="detail.state==3">
            <span class="ititle stoptitle">中止原因</span>
            <view class="stopreason">
                {{detail.origin.stopreason}}
            </view>
        </div>
        <i-button v-if="judgement&&!ratealready" type="primary" class="judge" @click="judgeToggle">评 价</i-button>
        <div class="mask" v-show="judgeShow" @click="judgeToggle">
            <!--        <div class="mask"   @click="judgeToggle">-->
            <div class="card" @click.stop=''>
                <view style="overflow: hidden">
                    <van-rate :value="rate" @change="onrateChange" count="3" size="28"
                              style='display: block;margin: 0 auto;width: 100%;text-align: center;margin-top: 52rpx;'/>
                    <div class="ratebox">
                        <span class="ratetxt">满意度调查：</span>
                        <span class="ratetxt">{{ratetxt}}</span>
                    </div>
                    <!--  <span :class="[{select:selectIndex==1},'col6', 'col1']" data-sid="1" @click.stop="judge">
                       <img src="/static/images/nice.png" class="nice jicon" alt="">
                       <img src="/static/images/niceactive.png" class="niceactive jicon" alt="">
                       <span class="jdtext">好评</span>
                   </span>
                   <span :class="[{select:selectIndex==2},'col6']" data-sid="2" @click.stop="judge">
                       <img src="/static/images/bad.png" class="bad jicon" alt="">
                       <img src="/static/images/badactive.png" class="badactive jicon" alt="">
                       <span class="jdtext">差评</span>
                   </span> -->
                </view>
                <view class="reasoniptbox" v-if="reasonShow">
                    <textarea :value="badReason" class="reasonipt" placeholder="请填写您的差评理由(200字以内)" maxlength="200"/>
                </view>
                <view class="submitbox">
                    <i-button type="primary" size="small" @click.stop="submit">提交</i-button>
                </view>
            </div>
        </div>
        <i-toast id="toast"/>
    </div>
</template>
<script>
    import {$Toast} from '../../../static/iview/base/index'

    export default {
        data() {
            return {
                detail: {
                    state: 2,
                    banzu: '',
                    gongzhang: '',
                    gzcell: '',
                    weixiugong: '',
                    wxgcell: '',
                    jubao: '0571-88888888',
                    location: '',
                    origin: {
                        danhao: '',
                        time: "",
                        name: "",
                        phone: "",
                        type: '',
                        content: [],
                        imgsUrl: [],
                        station: "",
                        address: "",
                        taidanhao: '',
                        stopreason: '',
                        EndTime: ''
                    },
                },
                Repairs: [],
                weixiuimgs: [],
                selectIndex: '',
                judgeShow: false,
                reasonShow: false,
                badReason: "",
                oid: '',
                rate: 3,//评分
                ratetxt: '满意',
                ratecode: 2,
                ratealready: false
            }
        },
        computed: {
            judgement: function () {
                console.log(this.detail.state);
                return (this.detail.state == 2 || this.detail.state == 3) ? true : false
            }
        },
        methods: {
            makeacall(e) {
                let wx = mpvue
                let number = e.mp.currentTarget.dataset.cell
                console.log(number)
                wx.makePhoneCall({
                    phoneNumber: number //仅为示例，并非真实的电话号码
                })
            },
            forNav: function () {
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
                _this.getPermission(_this).then(() => {
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
            getPermission: function (obj) {
                var wx = mpvue
                var pmse = new Promise((resolve, reject) => {
                    wx.getLocation({
                        type: 'gcj02', //返回可以用于wx.openLocation的经纬度
                        success: function (res) {
                            console.log(res);
                            obj.location = {
                                latitude: res.latitude,     //latitude,
                                longitude: res.longitude      //latitude
                            }
                            resolve()
                        },
                        fail: function () {
                            wx.getSetting({
                                success: function (res) {
                                    var statu = res.authSetting;
                                    if (!statu['scope.userLocation']) {
                                        wx.showModal({
                                            title: '是否授权当前位置',
                                            content: '需要获取您的地理位置，请确认授权，否则地图功能将无法使用',
                                            success: function (tip) {
                                                if (tip.confirm) {
                                                    wx.openSetting({
                                                        success: function (data) {
                                                            if (data.authSetting["scope.userLocation"] === true) {
                                                                $Toast({
                                                                    title: '授权成功',
                                                                    icon: 'success',
                                                                    duration: 1
                                                                })
                                                                //授权成功之后，再调用chooseLocation选择地方
                                                                wx.getLocation({
                                                                    type: 'gcj02', //返回可以用于wx.openLocation的经纬度
                                                                    success: function (res) {
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
                                fail: function (res) {
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
            preview: function (key) {
                wx.previewImage({
                    current: this.detail.origin.imgsUrl[key], // 当前显示图片的http链接
                    urls: this.detail.origin.imgsUrl  // 需要预览的图片http链接列表
                })
            },
            wxpreview: function (key) {
                wx.previewImage({
                    current: this.weixiuimgs[key], // 当前显示图片的http链接
                    urls: this.weixiuimgs  // 需要预览的图片http链接列表
                })
            },
            judgeToggle: function () {
                this.judgeShow = !this.judgeShow
            },
            judge: function (e) {
                var sid = e.mp.currentTarget.dataset.sid
                this.selectIndex = sid;
                if (sid == 1) {
                    this.reasonShow = false
                } else {
                    this.reasonShow = true
                }
            },
            onrateChange(e) {
                this.rate = e.mp.detail
                if (this.rate == 1) {
                    this.ratetxt = '不满意'
                    this.ratecode = 1
                } else if (this.rate == 2) {
                    this.ratetxt = '基本满意'
                    this.ratecode = 2
                } else if (this.rate == 3) {
                    this.ratetxt = '满意'
                    this.ratecode = 3
                }
                console.log(this.rate, this.ratetxt, this.ratecode)
            },
            submit() {
                var _this = this
                wx.request({
                    url: 'https://hd.xmountguan.com/railway/order.aspx?func=rate_order&uid=' + wx.getStorageSync('UID') + '&oid=' + _this.oid + '&rate=' + _this.ratecode,
                    success(res) {
                        console.log(res);
                        if (res.data.success) {
                            $Toast({
                                content: '评价成功',
                                type: 'success',
                                duration: 2,
                            });
                            _this.ratealready = true
                            _this.judgeShow = false
                        } else {
                            $Toast({
                                content: '请稍后重试',
                                type: 'warning'
                            });
                        }
                    }, fail() {
                        $Toast({
                            content: '请稍后重试',
                            type: 'warning'
                        });
                    }
                })
            }
        },
        created() {
        },
        mounted() {
            var _this = this
            this.oid = this.$root.$mp.query.oid;
            wx.request({
                url: 'https://hd.xmountguan.com/railway/order.aspx?func=get_order_detail&oid=' + this.oid, //仅为示例，并非真实的接口地址
                success(res) {

                    var Things = res.data
                    _this.detail.weixiugong = Things.RepairMan
                    _this.detail.origin.danhao = Things.SerialNo;
                    _this.detail.gongzhang = Things.Dealer;
                    _this.detail.gzcell = Things.DealerMobile;
                    _this.detail.origin.time = Things.CreateTime;
                    _this.detail.origin.type = Things.MaintenanceType;
                    _this.detail.origin.content = [Things.MaintenanceContentValue];
                    _this.detail.origin.imgsUrl = Things.Pictures;
                    _this.detail.origin.station = Things.Station;
                    _this.detail.origin.address = Things.DetailLocation;
                    _this.detail.origin.taidanhao = Things.TaidanNo;
                    _this.detail.origin.stopreason = Things.Process;
                    _this.detail.origin.EndTime = Things.EndTime;
                    _this.Repairs = Things.Repairs
                    console.log(Things.Rate);
                    console.log(_this.Repairs);

                    if (!(Things.Rate == "" || Things.Rate == null || typeof (Things.Rate) == "undefined")) {
                        _this.ratealready = true;
                        _this.rate = parseInt(Things.rate);
                        console.log("000");
                        if (Things.Rate == 1) {
                            console.log("111");
                            _this.ratetxt = '不满意'
                            _this.ratecode = 1
                        } else if (Things.Rate == 2) {
                            console.log("222");
                            _this.ratetxt = '基本满意'
                            _this.ratecode = 2
                        } else if (Things.Rate == 3) {
                            console.log("333");
                            _this.ratetxt = '满意'
                            _this.ratecode = 3
                        }
                    }
                    var statusText = Things.OrderStatus
                    var statuscode = ''
                    if (statusText == "待处理") {
                        statuscode = "0"
                    } else if (statusText == "维修中") {
                        statuscode = "1"
                    } else if (statusText == "已完成") {
                        statuscode = "2"
                    } else if (statusText == "已中止") {
                        statuscode = "3"
                    }
                    _this.detail.state = statuscode
                    _this.weixiuimgs = [];
                    setTimeout(() => {
                        for (var item of _this.Repairs) {
                            if (item.RepairPics.join('').length > 2) {
                                console.log(item);
                                for (var list of item.RepairPics) {
                                    if (list !== "" || list !== null || typeof (lsit) !== 'undefined') {
                                        _this.weixiuimgs.push(list)
                                    }
                                }
                            }
                        }
                    })
                }
            })


        },
        onload() {
            Object.assign(this.$data, this.$options.data())
        },
        onUnload() {
            console.log('onUnload', this)
            Object.assign(this.$data, this.$options.data())
        },
    }
</script>
<style>
    .gray {
        color: gray;
    }

    .yellow {
        color: #ff8c2e;
    }

    .green {
        color: green;
    }

    .red {
        color: #ed283c;
    }

    .ipts {
        min-height: 76rpx;
        line-height: 70rpx;
        font-size: 28rpx;
        display: block;
        margin: 0 auto;
        color: #3f4147;
        border-bottom: 1rpx solid rgba(222, 222, 222, 0.67);
        width: 100%;
        box-sizing: border-box;
        padding: 0 20rpx;
    }

    .ipts:nth-child(odd) {
        /*background: red;*/
    }

    .ipts:nth-child(even) {
        background: rgba(246, 255, 213, 0.07);
    }

    .ititle {
        display: block;
        float: left;
        padding-left: 10rpx;
    }

    .ript {
        display: block;
        height: 70rpx;
        line-height: 70rpx;
        float: right;
        text-align: right;
        padding-right: 10rpx;
        /*background: #ededed;*/
        width: 470rpx;
    }

    .phone {
        color: #2d8cf0;
    }

    .active .gou {
        display: inline;
    }

    .detailbox {
        width: 100%;
        padding: 20rpx;
        box-sizing: border-box;
        -webkit-box-sizing: border-box;
        background: rgba(246, 255, 213, 0.07);
    }

    .xqtxt {
        font-weight: bold;
        font-size: 30rpx
    }

    .detail {
        width: 100%;
        min-height: 600rpx;
        border: 1rpx solid rgba(128, 128, 128, 0.37);
        margin-top: 10rpx;
        padding: 10rpx;
        box-sizing: border-box;
        box-shadow: 0 0 10rpx rgba(92, 92, 92, 0.36);
        border-radius: 15rpx;
    }

    .bxlist {
        width: 100%;
        display: block;
        min-height: 65rpx;
        line-height: 65rpx;
        font-size: 28rpx;
        text-align: justify;
        padding-left: 10rpx;
        box-sizing: border-box;
    }

    .spans, .neirong {
        display: inline-block;
    }

    .neirong {
        border: 1rpx solid #b2b8c5;
        border-radius: 10rpx;
        margin: 0 10rpx 10rpx 10rpx;
        box-sizing: border-box;
        padding: 0 10rpx;
        font-size: 24rpx;
        height: 50rpx;
        display: inline-block;
        line-height: 50rpx;

    }

    .inspan {
        margin-right: 20rpx;
        text-align: left;
    }

    .imgbox {
        display: inline-block;
        width: 200rpx;
        height: 200rpx;
        /*background: rgba(170, 255, 119, 0.6);*/
        margin: 0 20rpx 0 0;
        position: relative;
        overflow: hidden;
    }

    .slt {
        position: absolute;
        width: 100%;
        height: auto;
        display: block;
        left: 0;
        top: 50%;
        transform: translateY(-50%);
        -webkit-transform: translateY(-50%);
    }

    .judge {
        margin-bottom: 150rpx;
    }

    .mask {
        display: block;
        width: 100vw;
        height: 100vh;
        position: fixed;
        left: 0;
        top: 0;
        background: rgba(0, 0, 0, 0.77);
        display: flex;
        flex-flow: column wrap;
        justify-content: center;
        align-items: center;
    }

    .card {
        width: 70%;
        /*min-height: 500rpx;*/
        display: block;
        background: white;
        border-radius: 20rpx;
        padding-bottom: 30rpx;
    }

    .col6 {
        width: 40%;
        height: 50rpx;
        /*background: rgba(28, 36, 56, 0.17);*/
        float: left;
        margin-top: 74rpx;
        line-height: 50rpx;
        text-align: center;
        padding: 0;
        box-sizing: border-box;
    }

    .col1 {
        margin-left: 10%;
    }

    .jicon {
        width: 50rpx;
        height: 50rpx;
        display: inline-block;
    }

    .jdtext {
        display: inline-block;
        font-size: 20rpx;
        margin-left: 10rpx;
        line-height: 21rpx;
        margin-top: -68rpx;
        position: relative;
        top: -14rpx;
    }

    .select .jdtext {
        color: rgb(18, 150, 219);
    }

    .niceactive, .badactive {
        display: none;
    }

    .select .niceactive, .select .badactive {
        display: inline-block;
    }

    .select .nice, .select .bad {
        display: none;
    }

    .stoptitle {
        display: block;
        float: none;
    }

    .stopreason {
        display: block;
        width: 97%;
        margin: 0 auto;
        padding: 20rpx;
        box-sizing: border-box;
        background: rgba(247, 244, 244, 0.59);
        /*border:1rpx solid gray;*/
        line-height: 34rpx;
        font-size: 27rpx;
    }

    .reasoniptbox {
        width: 81%;
        border: 1rpx solid #b2b2b2;
        display: block;
        margin: 0 auto;
        margin-top: 30rpx;
        height: 142rpx;
        box-sizing: border-box;
        padding: 10rpx;
    }

    .submitbox {
        width: 89%;
        display: block;
        margin: 0 auto;
        margin-top: 60rpx;
    }

    .reasonipt {
        width: 100%;
        height: 100%;
        text-align: justify;
        font-size: 23rpx;
        overflow: scroll;
    }

    .ratebox {
        text-align: center;
        margin-top: 30rpx;
    }
</style>
