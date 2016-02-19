<template>
  <button v-on:click="loadModule">Load</button>
  <div id="example"></div>
</template>

<script>

  var Vue = require('vue');
  var Q = require('q');

  var app = {
    data:function(){
      return {
        msg: 'Hello from vue-loader!'
      }
    },
    methods:{
      loadModule:function() {
        var link = Vue.component('link',function(){
          var deferred = Q.defer();
          require.ensure(["./qnupload.vue"], function(require) {
            var a = require("./components/qiniu-upload/qnupload.vue");
            deferred.resolve(a)
          });
          return deferred.promise;
        });
        link().then(function(res){
          new Vue(Object.assign({
            el:'#example'
          },res))
        });
      }
    }
  };
  module.exports = app;

</script>

<style>

</style>
