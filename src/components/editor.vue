<template>
  <div>
    <div ref="editor"></div>
  </div>
</template>

<script>
  import E from 'wangeditor';

  export default {
    props: ['content'],
    data () {
      return {
        editor: null,
        editorContent: ''
      }
    },
    mounted () {
      let editor = new E(this.$refs.editor);
      // 自定义菜单配置
      editor.customConfig.menus = [
        'head',  // 标题
        'bold',  // 粗体
        'fontSize',  // 字号
        'fontName',  // 字体
        'italic',  // 斜体
        'foreColor',  // 文字颜色
        'backColor',  // 背景颜色
        'link',  // 插入链接
        'list',  // 列表
        'justify',  // 对齐方式
        'quote',  // 引用
        'emoticon',  // 表情
        'image',  // 插入图片
        'table',  // 表格
        'code',  // 插入代码
        'undo',  // 撤销
        'redo'  // 重复
      ];
      // 忽略粘贴内容中的图片
      editor.customConfig.pasteIgnoreImg = true;
      // onchange 触发的延迟时间
      // editor.customConfig.onchangeTimeout = 500;
      // 限制一次最多上传 5 张图片
      editor.customConfig.uploadImgMaxLength = 5;
      // 隐藏"网络图片"tab
      editor.customConfig.showLinkImg = false;
      // 使用 base64 保存图片
      editor.customConfig.uploadImgShowBase64 = true;
      // 更改内容
      editor.customConfig.onchange = (html) => {
        this.editorContent = html;
      };
      editor.create();
      this.editor = editor;
    },
    methods: {
      getEditorContent () {
        this.$emit('sendEditorContent', this.editorContent);
      }
    },
    watch: {
      content (content) {
        this.editorContent = content;
        this.editor.txt.html(content);
      }
    }
  }
</script>

<style scoped>
  h1, h2 {
    font-weight: normal;
  }

  ul {
    list-style-type: none;
    padding: 0;
  }

  li {
    display: inline-block;
    margin: 0 10px;
  }

  a {
    color: #42b983;
  }
</style>
