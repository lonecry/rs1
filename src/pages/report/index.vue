<template>
    <div>
        <div class = "ipts">
            <span class = "ititle">报修人姓名</span>
            <input class = "ript" v-model = "value1" title = "报修人姓名" autofocus placeholder = "请输入姓名"/>
        </div>
        <div class = "ipts">
            <span class = "ititle">手机号</span>
            <input class = "ript" v-model = "value2" title = "报修人手机" type = "number" autofocus placeholder = "请输入手机号"/>
        </div>
        <div class = "ipts iptbox">
            <span class = "iboxtitle">*报修内容</span>
            <span @click = "selectErr" :class = "{active:currnetIndex==index}" :data-errkey = "index" :data-mid = 'item.errKey' class = "rptypes" v-for = "(item,index) in errors" :key = "index">{{item.errcontent}} <span class = "gou"><i-icon type = "flag_fill" size = "21" color = "red"/></span> </span>
        </div>
        <div class = "ipts">
            <span class = "ititle">*报修类型</span>
            <!--  <picker @change="bindPickerChange2" :value="errtypes" :range="errorsarray">
               <view class="ript ript3">
                   {{errtypes}}
               </view>
               <input class="ript ript3" v-model="value3" title="报修类型" autofocus disabled placeholder="报修类型"/>
           </picker> -->
            <view class = "ript ript3">
                {{errtypes}}
            </view>
        </div>
        <div class = "ipts iptbox">
            <span class = "iboxtitle">*现场图片(请拍摄附近参照物)</span>
            <div class = "uoloadimgs" :data-upid = "index" @click = "uploadImg" v-for = "(item,index) in upload" :key = "index">
                <span>+</span>
                <span class = "tips">上传图片</span>
                <div v-if = "item.show" class = "imgbox">
                    <img :src = "item.imgurl" :mode = "'widthFix'" @click.stop = "preview(index)" class = "uploadedimg">
                </div>
                <img v-if = "item.show" :data-upid = "index" src = "/static/images/delete.png" @click.stop = 'deleteImg' class = "delete">
            </div>
        </div>
        <div class = "ipts">
            <span class = "ititle">*选则车站</span>
            <!--    <div class="selocation" @click="searchstation">
                    <i-icon type="coordinates_fill" size="26" color="#2d8cf0" class="usericon" />
                    <span class="zhantai">{{zhantai}}</span>
                </div>-->
            <!--<input class = "ript" v-model = "value4" title = "选则车站" autofocus placeholder = "选则车站"/>-->
            <picker @change = "chehzanChange" :value = "chezhanindex" :range = "selectCheZhan" style = "text-align: right">
                <view class = "picker">
                    当前选择：{{selectCheZhan[chezhanindex]}}
                </view>
            </picker>
        </div>
        <div class = "ipts iptbox">
            <span class = "ititle">*详细位置描述</span>
            <textarea class = "reportDesc" v-model = "value5" type = "textarea" title = "详细位置描述" maxlength = "50" autofocus placeholder = "请输入报修描述"></textarea>
        </div>
        <div class = "ipts">
            <span class = "ititle">台单号(选填)</span>
            <input class = "ript" v-model = "value6" title = "台单号" autofocus placeholder = "台单号"/>
        </div>
        <div class = "submit">
            <i-button @click = "handleClick" type = "primary">提交报修</i-button>
        </div>
        <i-toast id = "toast"/>
        <van-popup :show = "slectType" position = "bottom">
            <view class = "check">
                <span class = "ckitem ckitem1" @click = "slectTypecancel">取消</span>
                <span class = "ckitem ckitem2" @click = "slectTypesure">确定</span>
            </view>
            <scroll-view scroll-y = "true" :style = "{ height: '460rpx'}">
                <i-panel>
                    <i-checkbox-group :current = "slectTypecurrent" :data-set = "slectTypecurrent" @change = "handleTypeChange">
                        <i-checkbox v-for = " (item,index) in slectType" :position = "'right'" :key = "index" :value = "item.name">
                        </i-checkbox>
                    </i-checkbox-group>
                </i-panel>
            </scroll-view>
        </van-popup>
        <i-modal :visible = "gologinShow" @ok = "gologin" @cancel = "logoincancel">
            <view>您还没有登录，确认登录吗？</view>
        </i-modal>
    </div>
</template>
<script>
    import {$Toast} from '../../../static/iview/base/index'

    export default {
        data(){
            return {
                addr                 : '',
                value1               : '朝夕',
                value2               : '13858000000',
                value3               : '水电问题',
                value4               : '输入框已禁用',
                value5               : '',
                value6               : '',
                value7               : '',
                currnetIndex         : '-1',
                errorTypes           : [],
                errors               : [
                    // {"errKey": "0 ", "errDetail": ''},
                ],
                errtypes             : '请点击报修内容',
                mid                  : '',
                uid                  : '',
                errorsarray          : [],
                array2               : ['修补了detail','不能修detail','不可归我管detail','其他detail'],
                upload               : [
                    {imgurl : '',show : false},
                    {imgurl : '',show : false},
                ],
                imgsUrl              : [],
                imgsId               : [],
                imgsforload          : '',
                m_c_value            : '',
                zhantai              : "点击选择站台",
                slectType            : false,
                selectCheZhanjson    : [],
                selectCheZhan        : [],
                chezhanindex         : 0,
                selectedCheZhanIndex : 0,
                gologinShow          : false
            }
        },
        methods : {
            chehzanChange : function(e){
                console.log(e);
                console.log('picker发送选择改变，携带值为',e.mp.detail.value)
                this.chezhanindex = e.mp.detail.value
                this.selectedCheZhanIndex = this.selectCheZhanjson[this.chezhanindex].sid
                console.log(this.selectedCheZhanIndex);
            },
            bindPickerChange2(e){
                var _this = this
                console.log('picker发送选择改变，携带值为',e.mp.detail.value)
                this.errtypes = this.errorTypes[e.mp.detail.value].errDetail
                _this.mid = this.errorTypes[e.mp.detail.value].errKey
                wx.request({
                    url : 'https://hd.xmountguan.com/railway/m.aspx?func=get_m_content_ddl&mid=' + _this.mid, //
                    success(res){
                        console.log(res.data)
                        _this.errors = []
                        for(var item of res.data){
                            _this.errors.push({"errKey" : item.MCID,"errcontent" : item.MaintenanceContent},)
                        }
                        console.log(_this.errors);
                    },
                    fail(){
                        console.log('网络错误')
                    }
                })
            },
            selectErr(e){
                var _this = this
                // console.log(e.mp.currentTarget.dataset.errkey);
                this.currnetIndex = e.mp.currentTarget.dataset.errkey
                this.m_c_value = this.errors[this.currnetIndex].errcontent
                console.log(this.currnetIndex,this.m_c_value)
                // https://hd.xmountguan.com/railway/m.aspx?func=get_m_ddl&mid=1、
                //
                //
                //
                wx.request({
                    url : 'https://hd.xmountguan.com/railway/m.aspx?func=get_m_ddl&mid=' + e.mp.currentTarget.dataset.mid,
                    success(res){
                        console.log(res)
                        console.log(res.data[0].mid);
                        _this.errtypes = res.data[0].maintenanceType
                        _this.mid = res.data[0].mid
                    },
                    fail(){
                        console.log('网络错误')
                    }
                })
            },
            preview       : function(k){
                var wx = mpvue
                var urls = []
                urls.push(this.imgsUrl[k])
                wx.previewImage({
                    current : urls[0], // 当前显示图片的http链接
                    urls    : urls // 需要预览的图片http链接列表
                })
            },
            uploadImg(e){
                var _this = this
                var indexOfLoad = e.mp.currentTarget.dataset.upid
                let wx = mpvue
                let i = 0; // 多图上传时使用到的index
                let that = this;
                let max = that.maxImg; //最大选择数
                let upLength; //图片数组长度
                let imgFilePaths; //图片的本地临时文件路径列表
                wx.chooseImage({
                    count      : max || 1, //一次最多可以选择的图片张数
                    sizeType   : ['original','compressed'], // 可以指定是原图还是压缩图，默认二者都有
                    sourceType : ['album','camera'], // 可以指定来源是相册还是相机，默认二者都有
                    success    : function(res){
                        // tempFilePath可以作为img标签的src属性显示图片
                        const tempFilePaths = res.tempFilePaths
                        // console.log(tempFilePaths);
                        _this.imgsUrl[indexOfLoad] = tempFilePaths[0]
                        _this.upload[indexOfLoad].imgurl = tempFilePaths
                        _this.upload[indexOfLoad].show = true
                        /**
                         * 上传完成后把文件上传到服务器
                         */
                        _this.fileUpload(tempFilePaths,indexOfLoad)
                        // $Toast({
                        //     content: '开始上传文件',
                        //     type: 'warning'
                        // });
                    },
                    fail       : function(){
                        console.log('fail');
                        $Toast({
                            content : '网络错误，请稍后重试',
                            type    : 'warning'
                        });
                    },
                    complete   : function(){
                        console.log('完成同步上传');
                    }
                })
            },
            deleteImg(e){
                var _this = this
                var indexOfLoad = e.mp.currentTarget.dataset.upid
                _this.upload[indexOfLoad].show = false
                _this.upload[indexOfLoad].imgurl = ''
                _this.imgsUrl[indexOfLoad] = ''
            },
            fileUpload    : function(tempFilePaths,index){
                console.log("待上传 ：" + tempFilePaths);
                var wx = mpvue
                var that = this;
                var _this = this
                wx.uploadFile({
                    url      : "https://hd.xmountguan.com/railway/upload_single_pic.aspx", //url地址， //app.ai_api.File.file
                    filePath : tempFilePaths.toString(), //要上传文件资源的路径 String类型
                    name     : 'uploadimg', //按个人情况修改，文件对应的 key,开发者在服务器端通过这个 key 可以获取到文件二进制内容，(后台接口规定的关于图片的请求参数)
                    header   : {
                        "Content-Type" : "multipart/form-data" //记得设置
                    },
                    formData : {
                        //和服务器约定的token, 一般也可以放在header中
                        // 'session_token': wx.getStorageSync('session_token')
                    },
                    success  : function(res){
                        //当调用uploadFile成功之后，再次调用后台修改的操作，这样才真正做了修改头像
                        if(res.statusCode = 200){
                            var data = JSON.parse(res.data)
                            // var statusCode = res.statusCode
                            // console.log("返回值1" + data);
                            // console.log("返回值2" + statusCode)
                            //这里调用后台的修改操作， tempFilePaths[0],是上面uploadFile上传成功，然后赋值到修改这里。
                            console.log(data);
                            console.log("文件路径" + data.imgurl);
                            _this.imgsUrl[index] = data.imgurl
                            _this.upload[index].imgurl = data.imgurl
                            _this.imgsId[index] = data.imgID
                            console.log("待提交文件",_this.upload)
                            // $Toast({
                            //     content: '上传成功',
                            //     type: 'success',
                            //     duration: 2,
                            // });
                        }
                    },
                    fail(e){
                        console.log(e);
                        $Toast({
                            content : '网络错误，请稍后重试',
                            type    : 'warning'
                        });
                    }
                })
            },
            gologin(){
                this.gologinShow = false
                mpvue.navigateTo({
                    url : '../login/main',
                })
            },
            logoincancel(){
                this.gologinShow = false
            }
            ,
            handleClick(){
                var login = mpvue.getStorageSync('login')
                if(!login){
                    /* wx.redirectTo({
                     // url: '../detailforworker/main',
                     url: '../login/main',
                     })*/
                    this.gologinShow = true
                } else {
                    var wx = mpvue;
                    var _this = this
                    // console.log("clicked")
                    //
                    // _this.imgsUrl[indexOfLoad] = tempFilePaths[0]
                    // for (var item in  _this.imgsUrl){
                    //     _this.fileUpload(_this.imgsUrl[item], item)
                    // }
                    var flg1 = this.imgsId[0]
                    var flg2 = this.imgsId[1]
                    var imgsidforload = ''
                    if(flg1 && flg2){
                        imgsidforload = this.imgsId.join(',')
                    } else if(flg1){
                        imgsidforload = this.imgsId[0]
                    } else if(flg2){
                        imgsidforload = this.imgsId[1]
                    }
                    var json = {
                        "ordertype"      : 1,
                        "sid"            : _this.selectedCheZhanIndex, //写死
                        "detaillocation" : this.value5,
                        "taidanno"       : _this.value6,
                        "mid"            : _this.mid,
                        "m_c_value"      : this.m_c_value,
                        "uid"            : _this.uid,
                        "pictures"       : imgsidforload
                    }
                    var urlafter = ""
                    for(var key in json){
                        urlafter = urlafter + "&" + key + '=' + json[key]
                    }
                    console.log(urlafter);
                    if(json.ordertype == "" || json.sid == "" || json.detaillocation == "" || json.mid == "" || json.m_c_value == "" || json.uid == "" || json.pictures == ""){
                        //输入校验
                        $Toast({
                            content : '请完善输入',
                            type    : 'warning'
                        });
                    } else {

                        //提交
                        wx.request({
                            url : 'https://hd.xmountguan.com/railway/order.aspx?func=add_order_auto&' + urlafter,
                            success(res){
                                console.log(res.data)
                                if(res.data.oid){
                                    $Toast({
                                        content  : '上传成功',
                                        type     : 'success',
                                        duration : 2,
                                    });
                                    // wx.redirectTo({
                                    //     url: '../indexswiper/main'
                                    // })
                                    wx.reLaunch({
                                        url : '../indexswiper/main'
                                    })
                                }
                            },
                            fail(){
                                console.log('网络错误')
                                $Toast({
                                    content : '网络错误，请稍后重试',
                                    type    : 'warning'
                                });
                            }
                        })
                    }
                }
            },
            searchstation : function(){
                var _this = this
                _this.getPermission(_this).then(() =>{
                    wx.navigateTo({
                        url : '../locationSearch/main',
                    })
                    var destination = {
                        latitude  : _this.data.latitude,
                        longitude : _this.data.longitude,
                        name      : "中国浙江省杭州市西湖区莫干山路111号",
                        address   : "浙江省杭州市拱墅区米市巷街道半道红社区西南方向",
                        scale     : 18
                    }
                    wx.setStorageSync('destination',destination)
                })
            },
            getPermission : function(obj){
                var wx = mpvue
                var pmse = new Promise((resolve,reject) =>{
                    wx.getLocation({
                        type    : 'gcj02', //返回可以用于wx.openLocation的经纬度
                        success : function(res){
                            console.log("授权成功",res);
                            wx.setStorageSync('location',res)
                            obj.location = {
                                latitude  : res.latitude, //latitude,
                                longitude : res.longitude //latitude
                            }
                            resolve()
                        },
                        fail    : function(){
                            wx.getSetting({
                                success : function(res){
                                    var statu = res.authSetting;
                                    if(!statu['scope.userLocation']){
                                        wx.showModal({
                                            title   : '是否授权当前位置',
                                            content : '需要获取您的地理位置，请确认授权，否则地图功能将无法使用',
                                            success : function(tip){
                                                if(tip.confirm){
                                                    wx.openSetting({
                                                        success : function(data){
                                                            if(data.authSetting["scope.userLocation"] === true){
                                                                $Toast({
                                                                    title    : '授权成功',
                                                                    icon     : 'success',
                                                                    duration : 1
                                                                })
                                                                //授权成功之后，再调用chooseLocation选择地方
                                                                wx.getLocation({
                                                                    type    : 'gcj02', //返回可以用于wx.openLocation的经纬度
                                                                    success : function(res){
                                                                        console.log("授权成功" + res);
                                                                        wx.setStorageSync('location',res)
                                                                        obj.location = {
                                                                            latitude  : res.latitude, //latitude,
                                                                            longitude : res.longitude //latitude
                                                                        }
                                                                        resolve()
                                                                    },
                                                                })
                                                            } else {
                                                                $Toast({
                                                                    title    : '授权失败',
                                                                    icon     : 'success',
                                                                    duration : 1
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
                                fail    : function(res){
                                    $Toast({
                                        title    : '调用授权窗口失败',
                                        icon     : 'success',
                                        duration : 1
                                    })
                                    reject()
                                }
                            })
                        }
                    })
                })
                return pmse
            },
        },
        mounted : function(){
            console.log('mounted')
            this.zhantai = mpvue.getStorageSync("addr") || this.zhantai
            console.log(this.zhantai);
            var wx = mpvue;
            var _this = this
            _this.uid = wx.getStorageSync('UID');
            this.value1 = wx.getStorageSync("UserName")
            this.value2 = wx.getStorageSync("Mobile")
            wx.request({
                url : 'https://hd.xmountguan.com/railway/m.aspx?func=get_m_ddl',
                success(res){
                    _this.errorTypes = []
                    for(var item of res.data){
                        console.log(item);
                        _this.errorTypes.push({"errKey" : item.mid,"errDetail" : item.maintenanceType},)
                        _this.errorsarray.push(
                            item.maintenanceType,
                        )
                    }
                    console.log(_this.errors);
                },
                fail(){
                    console.log('网络错误')
                }
            })
            wx.request({
                url : 'https://hd.xmountguan.com/railway/m.aspx?func=get_m_content_ddl&mid=0', //
                success(res){
                    console.log(res.data)
                    _this.errors = []
                    for(var item of res.data){
                        _this.errors.push({"errKey" : item.MCID,"errcontent" : item.MaintenanceContent},)
                    }
                    console.log(_this.errors);
                },
                fail(){
                    console.log('网络错误')
                }
            })
            //车站下拉
            wx.request({
                url : 'https://hd.xmountguan.com/railway/station.aspx?func=get_wx_station_ddl', //
                success(res){
                    console.log(res.data)
                    _this.selectCheZhanjson = res.data
                    for(var i = 0 ; i < res.data.length ; i++){
                        _this.selectCheZhan.push(res.data[i].sname)
                    }
                },
                fail(){
                    console.log('网络错误')
                }
            })
            // https://hd.xmountguan.com/railway/station.aspx?func=get_wx_station_ddl
        },
        onshow(){
            console.log('show')
        },
    }
</script>
<style>
    .ipts {
        width: 740rpx;
        height: 70rpx;
        line-height: 70rpx;
        font-size: 28rpx;
        display: block;
        margin: 0 auto;
        color: #3f4147;
        border-bottom: 1rpx solid rgba(222, 222, 222, 0.67);
    }

    .submit {
        width: 740rpx;
        height: 100rpx;
        margin-top: 100rpx;
        position: relative;
        margin-bottom: 200rpx;
    }

    .ititle {
        display: block;
        float: left;
        padding-left: 10rpx;
    }

    .iboxtitle {
        display: block;
        /*float        : left;*/
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
        width: 525rpx;
        box-sizing: border-box;
        padding: 0 20rpx;
    }

    .ript3 {
        /*color : #ff0762;*/
    }

    .iptbox {
        height: auto;
    }

    .rptypes {
        width: auto;
        height: 57rpx;
        display: inline-block;
        margin: 8rpx;
        padding: 0 25rpx;
        border-radius: 20rpx;
        border: 1rpx solid #5e6268;
        color: #5e6268;
        line-height: 57rpx;
        font-size: 23rpx;
    }

    .gou {
        display: none;
    }

    .active {
        border: 1rpx solid #4696ff;
        color: #4696ff;
    }

    .active .gou {
        display: inline;
    }

    .uoloadimgs {
        display: inline-block;
        width: 150rpx;
        text-align: center;
        line-height: 107rpx;
        font-size: 78rpx;
        height: 150rpx;
        border: 2rpx solid #cbcbcb;
        background: #f3f3f3;
        margin: 10rpx 18rpx;
        /*border-radius : 16rpx;*/
        color: #d0d0d0;
        position: relative;
    }

    .tips {
        position: absolute;
        width: 100%;
        text-align: center;
        left: 0;
        top: 57rpx;
        font-size: 20rpx;
        color: #d0d0d0;
    }

    .imgbox {
        width: 100%;
        height: 100%;
        position: absolute;
        left: 0;
        top: 0;
        overflow: hidden;
    }

    .uploadedimg {
        position: absolute;
        left: 0;
        top: 50%;
        width: 100%;
        /*height   : 100%;*/
        z-index: 1;
        transform: translateY(-50%);
        -webkit-transform: translateY(-50%);
        background: white;
    }

    .delete {
        position: absolute;
        right: -18rpx;
        top: -18rpx;
        width: 36rpx;
        height: 36rpx;
        z-index: 2;
    }

    .selocation {
        width: 242rpx;
        float: right;
        height: 100%;
        text-align: right;
        padding-right: 10rpx;
    }

    .reportDesc {
        display: inline-block;
        width: 97%;
        text-align: justify;
        line-height: 30rpx;
        font-size: 28rpx;
        height: 150rpx;
        border: 2rpx solid #cbcbcb;
        background: #f3f3f3;
        color: #5c5c5c;
        position: relative;
        box-sizing: border-box;
        padding: 14rpx;
        margin: 0 auto;
        display: block;
        margin-top: 20rpx;
        margin-bottom: 20rpx;
    }
</style>
