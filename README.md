# Image_upload_aliyun
微信小程序上传图片到阿里云oss

项目是demo，可以较为清晰的查看到使用方法，为了更加简洁明了的给各位开发者呈现使用，具体的接入使用步骤如下：

1、将demo中utils文件夹下的js文件复制到自己项目的utils文件夹下

2、配置utils文件夹下的config.js文件内容：

例子：

var fileHost = 'http://4zlinktest.oss-cn-beijing.aliyuncs.com/';//你的阿里云地址最后面跟上一个/   在你当前小程序的后台的uploadFile 合法域名也要配上这个域名
var config = {
//aliyun OSS config
uploadImageUrl: `${fileHost}`, // 默认存在根目录，可根据需求改
AccessKeySecret: 'zmOJaqKJB54gqtLunHcNoMBTdDgp',        // AccessKeySecret 去你的阿里云上控制台上找
OSSAccessKeyId: 'LTAIPJL9J5OZq2G',         // AccessKeyId 去你的阿里云上控制台上找
timeout: 87600 //这个是上传文件时Policy的失效时间
};
module.exports = config

3、在自己需要使用上传功能中的js文件中，引入工具文件并设置上传路径：

//index.js
//获取应用实例
const app = getApp()
var uploadImage = require('../../utils/uploadFile.js');
var util = require('../../utils/util.js');

Page({
data: {

},

//选择照片
chooseAndUpload:function(){
wx.chooseImage({
count: 6, // 默认最多一次选择9张图
sizeType: ['original', 'compressed'], // 可以指定是原图还是压缩图，默认二者都有
sourceType: ['album', 'camera'], // 可以指定来源是相册还是相机，默认二者都有
success: function (res) {
// 返回选定照片的本地文件路径列表，tempFilePath可以作为img标签的src属性显示图片
var tempFilePaths = res.tempFilePaths;
var nowTime = util.formatTime(new Date());

//支持多图上传
for (var i = 0; i < res.tempFilePaths.length; i++) {
//显示消息提示框
wx.showLoading({
title: '上传中' + (i + 1) + '/' + res.tempFilePaths.length,
mask: true
})

//上传图片
//你的域名下的/mifanimg文件下的/当前年月日文件下的/图片.png
//图片路径可自行修改
uploadImage(res.tempFilePaths[i], 'mifanimg/' + nowTime + '/',
function (result) {
console.log("======上传成功图片地址为：", result);
wx.hideLoading();
}, function (result) {
console.log("======上传失败======", result);
wx.hideLoading()
}
)
}
}
})
}
})

4、图片上传成功后会有返回地址，将地址传到业务后台即可，至此就完成了整个上传过程。
备注：如在使用过程中遇到问题，可提出到issue中或者加qq：1203503422



