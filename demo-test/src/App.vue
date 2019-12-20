<template>
  <div id="app">
   
    <div>
      <Ueditor @ready="editorReady" @setContent='get' style="width: 1000px;height: 400px;margin:0px auto;"></Ueditor>
    </div>
    <div>
      <HelloWorld :msg="content"/>
    </div>
  </div>
</template>
<script>
  import Ueditor from './components/Ueditor'
  import HelloWorld from './components/HelloWorld'
  export default {
    data(){
      return{
        content:''
      }
    },
    name: 'app',
    components: {
      Ueditor,
      HelloWorld
    },
    methods: {
      editorReady (instance) {
        instance.setContent('');
        instance.addListener('contentChange', () => {
          this.content = instance.getContent();
        });
      },
      get(val){
          this.content = val
      }
    },
  }
</script>
