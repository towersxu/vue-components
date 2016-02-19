<template>
  <video id="vid1" class="video-js gaia show-control vjs-default-skin " controls preload="auto" width="860" height="482" poster="img/B3.jpg">
    <p>Video Playback Not Supported</p>
  </video>
</template>

<script>
  require('./video-js.css');
  require('./modernizr.js');
  require('./video.js');
  require('./videojs.hls.js');
  module.exports = {
    ready:function(){
      console.log(videojs)
      var vid1, progressed,volumeMenuButton;

      // create a really simple plugin
      // this one just logs the buffered percentage to the console whenever
      // more data is downloaded
      progressed = function(options) {
//      this.on('progress', function(e) {
//        console.log(this.bufferedPercent());
//      });
      };

      // register the plugin
      videojs.plugin('progressed', progressed);

      // initialize it
      vid1 = videojs('vid1',{
        controlBar: {
          volumeMenuButton: {
            inline: false,
            vertical: true
          },
          PlayResolutions:{
            idx:2,   //0-n
            videos:[
              {
                name:'2K',
                src:'http://7xqokl.com2.z0.glb.qiniucdn.com/14kaestrue.m3u8',
                type: 'application/vnd.apple.mpegurl'
              },
              {
                name:'4K',
                src:'http://7xqokl.com2.z0.glb.qiniucdn.com/14kaestrue.m3u8',
                type: 'application/vnd.apple.mpegurl'
              },
              {
                name:'AES',
                src:'./m3u8/aes128.m3u8',
                type: 'application/vnd.apple.mpegurl'
              },
              {
                name:'1080p',
                src:'./m3u8/1080.m3u8',
                type: 'application/vnd.apple.mpegurl'
              },
              {
                name:'720p',
                src:'./m3u8/720.m3u8',
                type: 'application/vnd.apple.mpegurl'
              },
              {
                name:'480p',
                src:'./m3u8/480.m3u8',
                type: 'application/vnd.apple.mpegurl'
              }
            ]
          },
          PlayerRatio:{
            idx:1,
            ratios:[
              {
                name:'16:9',
                width:'860',
                height:'483'
              },
              {
                name:'4:3',
                width:'860',
                height:'645'
              }
            ]
          },
          LogoText:{
            click:function(){
              console.log('this is logo');
            }
          }
        }
      });
    }
  }
</script>
