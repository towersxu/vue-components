<style scoped>
  #qnupload-container{}
  #qnupload-btn{
    width: 145px;
    border-radius: 3px;
    -webkit-box-shadow: 0 1px 0 rgba(243, 243, 243, 0.75);
    box-shadow: 0 1px 0 rgba(243, 243, 243, 0.75);
    color: #646464;
    border: 1px solid #d2d2d2;
    text-align: center;
    -webkit-transition: background-color 0.218s, border-color 0.218s, box-shadow 0.218s;
    transition: background-color 0.218s, border-color 0.218s, box-shadow 0.218s;
    -webkit-user-select: none;
    outline: none;
    padding: 10px 16px;
    font-size: 18px;
    line-height: 1.3333333;
    display: inline-block;
    margin-bottom: 0;
    font-weight: normal;
    white-space: nowrap;
    vertical-align: middle;
    touch-action: manipulation;
    cursor: pointer;
    user-select: none;
    background-image: none;
    text-decoration: none;
  }
</style>

<template>
  <div id="qnupload-container">
    <a href="#" id="qnupload-btn"><span>选择文件</span></a>
  </div>
</template>

<script>
  var moxie = require('./moxie.js');
  var plup = require('./plupload.dev.js');
  var qinniu = require('./qiniu.js');
  module.exports = {
    ready:function(){
      console.log(document.getElementById('qnupload-btn'));
      var uploader = Qiniu.uploader({
        idx:123,
        runtimes: 'html5,flash,html4',    //上传模式,依次退化
        browse_button: 'qnupload-btn',       //上传选择的点选按钮，**必需**
        uptoken_url: '/token',            //Ajax请求upToken的Url，**强烈建议设置**（服务端提供）
        // uptoken : '<Your upload token>', //若未指定uptoken_url,则必须指定 uptoken ,uptoken由其他程序生成
        // unique_names: true, // 默认 false，key为文件名。若开启该选项，SDK为自动生成上传成功后的key（文件名）。
        // save_key: true,   // 默认 false。若在服务端生成uptoken的上传策略中指定了 `sava_key`，则开启，SDK会忽略对key的处理
        domain: 'http://qiniu-plupload.qiniudn.com/',   //bucket 域名，下载资源时用到，**必需**
        get_new_uptoken: false,  //设置上传文件的时候是否每次都重新获取新的token
        container: 'qnupload-container',           //上传区域DOM ID，默认是browser_button的父元素，
        max_file_size: '100mb',           //最大文件体积限制
        flash_swf_url: 'js/plupload/Moxie.swf',  //引入flash,相对路径
        max_retries: 3,                   //上传失败最大重试次数
        dragdrop: true,                   //开启可拖曳上传
        drop_element: 'qnupload-container',        //拖曳上传区域元素的ID，拖曳文件或文件夹后可触发上传
        chunk_size: '4mb',                //分块上传时，每片的体积
        auto_start: true,                 //选择文件后自动上传，若关闭需要自己绑定事件触发上传
        init: {
          'FilesAdded': function(up, files) {
            plupload.each(files, function(file) {
              // 文件添加进队列后,处理相关的事情
            });
          },
          'BeforeUpload': function(up, file) {
            // 每个文件上传前,处理相关的事情
          },
          'UploadProgress': function(up, file) {
            // 每个文件上传时,处理相关的事情
          },
          'FileUploaded': function(up, file, info) {
            // 每个文件上传成功后,处理相关的事情
            // 其中 info 是文件上传成功后，服务端返回的json，形式如
            // {
            //    "hash": "Fh8xVqod2MQ1mocfI4S4KpRL6D98",
            //    "key": "gogopher.jpg"
            //  }
            // 参考http://developer.qiniu.com/docs/v6/api/overview/up/response/simple-response.html

            // var domain = up.getOption('domain');
            // var res = parseJSON(info);
            // var sourceLink = domain + res.key; 获取上传成功后的文件的Url
          },
          'Error': function(up, err, errTip) {
            //上传出错时,处理相关的事情
          },
          'UploadComplete': function() {
            //队列文件处理完毕后,处理相关的事情
          },
          'Key': function(up, file) {
            // 若想在前端对每个文件的key进行个性化处理，可以配置该函数
            // 该配置必须要在 unique_names: false , save_key: false 时才生效

            var key = "";
            // do something with key here
            return key
          }
        }
      });
    }
  };
</script>
