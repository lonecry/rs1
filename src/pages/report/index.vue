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
        <div class = "ipts">
            <span class = "ititle">*报修类型</span>
            <input class = "ript ript3" v-model = "value3" title = "报修类型" autofocus disabled placeholder = "报修类型"/>
        </div>
        <div class = "ipts iptbox">
            <span class = "iboxtitle">*报修类型</span>
            <span @click = "selectErr" :class = "{active:currnetIndex==index}" :data-errkey = "item.errKey" class = "rptypes" v-for = "(item,index) in errors" :key = "index">{{item.errDetail}} <span class = "gou">√</span> </span>
        </div>
        <div class = "ipts iptbox">
            <span class = "iboxtitle">*现场图片(请拍摄附近参照物)</span>
            <div class = "uoloadimgs" :data-upid = "index" @click = "uploadImg" v-for = "(item,index) in upload" :key = "index">
                <span>+</span>
                <span class = "tips">上传图片</span>
                <div v-if = "item.show" class = "imgbox">
                    <img :src = "item.imgurl" :mode = "'widthFix'" @click.stop = 'preview(item.imgurl)' class = "uploadedimg">
                </div>
                <img v-if = "item.show" :data-upid = "index" src = "/static/images/delete.png" @click.stop = 'deleteImg' class = "delete">
            </div>
        </div>
        <div class = "ipts">
            <span class = "ititle">*选则车站</span>
            <div class = "selocation" @click = "searchstation">
                <i-icon type = "coordinates_fill" size = "26" color = "#2d8cf0" class = "usericon"/>
                <span class = "zhantai">{{zhantai}}</span>
            </div>
            <!--<input class = "ript" v-model = "value4" title = "选则车站" autofocus placeholder = "选则车站"/>-->
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
    </div>
</template>
<script>
    export default {
        data(){
            return {
                addr: '',
                value1: '朝夕',
                value2: '13858000000',
                value3: '水电问题',
                value4: '输入框已禁用',
                value5: '',
                value6: '',
                value7: '',
                currnetIndex: '-1',
                errors: [
                    {"errKey": "0 ", "errDetail": '水表坏'},
                    {"errKey": "1 ", "errDetail": '水表坏'},
                    {"errKey": "2 ", "errDetail": '水表块了'},
                    {"errKey": "3 ", "errDetail": '水表满了'},
                    {"errKey": "4 ", "errDetail": '水表不转'},
                    {"errKey": "5 ", "errDetail": '水表不准'},
                    {"errKey": "6 ", "errDetail": '水表指针不见了'},
                ],
                upload: [
                    {imgurl: '', show: false},
                    {imgurl: '', show: false},
                ],
                imgsUrl: [],
                zhantai: "点击选择站台",
            }
        },
        methods: {
            selectErr(e){
                // console.log(e.mp.currentTarget.dataset.errkey);
                this.currnetIndex = e.mp.currentTarget.dataset.errkey
            },
            preview: function (url){
                var arr=["http://www.simpleqq.com/index/imgs/tab/tab4.jpg"]
                // arr.push(url)
                wx.previewImage({
                    current: arr[0], // 当前显示图片的http链接
                    urls: arr // 需要预览的图片http链接列表
                })
            },
            uploadImg(e){
                var _this = this
                var indexOfLoad = e.mp.currentTarget.dataset.upid
                let wx = mpvue
                console.log("uploadImg")
                let i = 0;					// 多图上传时使用到的index
                let that = this;
                let max = that.maxImg;		//最大选择数
                let upLength;						//图片数组长度
                let imgFilePaths;				//图片的本地临时文件路径列表
                wx.chooseImage({
                    count: max || 1,           //一次最多可以选择的图片张数
                    sizeType: ['original', 'compressed'], // 可以指定是原图还是压缩图，默认二者都有
                    sourceType: ['album', 'camera'], // 可以指定来源是相册还是相机，默认二者都有
                    success: function (res){
                        // tempFilePath可以作为img标签的src属性显示图片
                        const tempFilePaths = res.tempFilePaths
                        _this.imgsUrl[indexOfLoad] = tempFilePaths
                        _this.upload[indexOfLoad].imgurl = tempFilePaths
                        _this.upload[indexOfLoad].show = true
                        /**
                         * 上传完成后把文件上传到服务器
                         */
                        // wx.showLoading({
                        //     title: '上传中...',
                        // })
                        // _this.fileUpload(imgFilePaths, i, upLength);			//上传操作
                    },
                    fail: function (){
                        console.log('fail');
                    },
                    complete: function (){
                        console.log('commplete');
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
            fileUpload: function (tempFilePath){

                /*const uploadTask = wx.uploadFile({
                    // .....
                })
                uploadTask.onProgressUpdate((res) => {
                    console.log('上传进度', res.progress)
                    console.log('已经上传的数据长度', res.totalBytesSent)
                    console.log('预期需要上传的数据总长度', res.totalBytesExpectedToSend)
                })*/


                var _this = this
                let wx = mpvue
                wx.uploadFile({
                    url: "",//url地址， //app.ai_api.File.file
                    filePath: tempFilePath,  //文件路径  这里是mp3文件
                    name: 'file',  //随意
                    header: {
                        'Content-Type': 'multipart/form-data',
                        'Authorization': wx.getStorageSync("access_token"),  //如果需要token的话要传
                    },
                    formData: {
                        method: 'POST'   //请求方式
                    },
                    success(res){
                        var data = JSON.parse(res.data)  // 坑2：与wx.request不同的是，upload返回的是字符串格式，需要字符串对象化
                        if (data.code == 200) {
                            that.fileTrans(data.data.id); //执行接口函数 语音文件转文字
                        } else {
                            console.log('上传失败')
                            wx.showToast({
                                title: res.message,
                                icon: 'none'
                            })
                        }
                    }
                })
            },
            handleClick(){
                console.log("clicked")
            },
            searchstation(){
                mpvue.navigateTo({
                    url: '../locationSearch/main',
                })
            }
        },
        created(){
        },
        onShow: function (){
            this.zhantai = mpvue.getStorageSync("addr") || this.zhantai

            console.log(this.zhantai);
        },
    }
</script>
<style>
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
    .submit {
        width         : 740rpx;
        height        : 100rpx;
        margin-top    : 100rpx;
        position      : relative;
        margin-bottom : 200rpx;

    }
    .ititle {
        display      : block;
        float        : left;
        padding-left : 10rpx;
    }
    .iboxtitle {
        display      : block;
        /*float        : left;*/
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
    .ript3 {
        color : #ff0762;
    }
    .iptbox {

        height : auto;

    }
    .rptypes {
        width         : auto;
        height        : 57rpx;
        display       : inline-block;
        margin        : 8rpx;
        padding       : 0 25rpx;
        border-radius : 20rpx;
        border        : 1rpx solid #5e6268;
        color         : #5e6268;
        line-height   : 57rpx;
        font-size     : 23rpx;
    }
    .gou {
        display : none;
    }
    .active {
        border : 1rpx solid #4696ff;
        color  : #4696ff;
    }
    .active .gou {
        display : inline;
    }
    .uoloadimgs {
        display     : inline-block;
        width       : 150rpx;
        text-align  : center;
        line-height : 107rpx;
        font-size   : 78rpx;
        height      : 150rpx;
        border      : 2rpx solid #cbcbcb;
        background  : #f3f3f3;
        margin      : 10rpx 18rpx;
        /*border-radius : 16rpx;*/
        color       : #d0d0d0;
        position    : relative;

    }
    .tips {
        position   : absolute;
        width      : 100%;
        text-align : center;
        left       : 0;
        top        : 57rpx;
        font-size  : 20rpx;
        color      : #d0d0d0;

    }
    .imgbox {
        width    : 100%;
        height   : 100%;
        position : absolute;
        left     : 0;
        top      : 0;
        overflow : hidden;
    }
    .uploadedimg {
        position          : absolute;
        left              : 0;
        top               : 50%;
        width             : 100%;
        /*height   : 100%;*/
        z-index           : 1;
        transform         : translateY(-50%);
        -webkit-transform : translateY(-50%);

    }
    .delete {
        position : absolute;
        right    : -18rpx;
        top      : -18rpx;
        width    : 36rpx;
        height   : 36rpx;
        z-index  : 2;

    }
    .selocation {
        width         : 242rpx;
        float         : right;
        height        : 100%;
        text-align    : right;
        padding-right : 10rpx;

    }
    .reportDesc {
        display     : inline-block;
        width       : 100%;
        text-align  : justify;
        line-height : 30rpx;
        font-size   : 28rpx;
        height      : 150rpx;
        border      : 2rpx solid #cbcbcb;
        background  : #f3f3f3;
        color       : #5c5c5c;
        position    : relative;
        box-sizing  : border-box;
        padding     : 10rpx;
    }
</style>
