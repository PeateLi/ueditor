<template>
  <div>
    <!--下面通过传递进来的id完成初始化-->
    <script :id="randomId" style="height:100%"  type="text/plain"></script>
    <div style="margin-top:20px;">
        <el-button type="primary" @click="getText()">获取文本内容</el-button>
        <el-button type="primary" @click="getContent()">获取内容</el-button>
        <el-button type="primary" @click="getActiveTxt()">获取选中的文本</el-button>
    </div>
     <div  style="margin-top:20px;">
       <el-button @click="set()">发送内容</el-button>
     </div>

  </div>
</template>

<script>
  //需要修改  ueditor.config.js 的路径
  //var URL = window.UEDITOR_HOME_URL || ‘/static/ueditor_1/‘;
  //主体文件引入
  import '../../static/ueditor/ueditor.config.js'
  import '../../static/ueditor/ueditor.all.min.js'
  import '../../static/ueditor/lang/zh-cn/zh-cn.js'

  export default {
    props: {
      //配置可以传递进来
      ueditorConfig:{
        
      }
    },
    data () {
      return {
        //每个编辑器生成不同的id,以防止冲突
        randomId: 'editor_' + (Math.random() * 100000000000000000),
        //编辑器实例
        instance: null,
      };
    },
    //此时--el挂载到实例上去了,可以初始化对应的编辑器了
    mounted () {
      this.initEditor()
    },

    beforeDestroy () {
      // 组件销毁的时候，要销毁 UEditor 实例
      if (this.instance !== null && this.instance.destroy) {
        this.instance.destroy();
      }
    },
    methods: {
      initEditor () {
        //dom元素已经挂载上去了
          this.$nextTick(() => {
            this.instance = UE.getEditor(this.randomId, this.ueditorConfig);
            // 绑定事件，当 UEditor 初始化完成后，将编辑器实例通过自定义的 ready 事件交出去
            this.instance.addListener('ready', () => {
              this.$emit('ready', this.instance);
            });
          });
        },
        getText(){    //获取文本不带标签
          console.log( this.instance.getContentTxt())
        },
        getContent(){ //获取文本带标签
          console.log( this.instance.getContent())
        },
        getActiveTxt(){  //获取选中的文本
          //当你点击按钮时编辑区域已经失去了焦点，如果直接用getText将不会得到内容，所以要在选回来，然后取得内容
           var range = this.instance.selection.getRange();
          range.select();
          var txt = this.instance.selection.getText();
        },
        set(){
          console.log(this.instance.getContent())
          var txt = this.instance.getContent()
          this.$emit('setContent',txt)
        }

    }
  };
</script>