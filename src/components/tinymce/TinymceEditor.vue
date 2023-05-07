<script lang="ts" setup>
import { computed, onMounted, onUnmounted } from 'vue';

import tinymce from 'tinymce/tinymce'; //tinymce核心文件
// import Editor from '@tinymce/tinymce-vue';

import './models/dom/model.min.js'; // 引入dom模块。从 Tinymce6，开始必须有此模块导入
import './themes/silver/theme.min.js'; //默认主题
import './icons/default/icons.min.js'; //引入编辑器图标icon，不引入则不显示对应图标
import './langs/zh-Hans.js'; //引入编辑器语言包
import './skins/content/default/content.min.css'
import './skins/ui/oxide/skin.min.css'

import './plugins/advlist/plugin.min.js'; //高级列表
import './plugins/anchor/plugin.min.js'; //锚点
import './plugins/autolink/plugin.min.js'; //自动链接
import './plugins/autoresize/plugin.min.js'; //编辑器高度自适应,注：plugins里引入此插件时，Init里设置的height将失效
import './plugins/autosave/plugin.min.js'; //自动存稿
import './plugins/charmap/plugin.min.js'; //特殊字符
import './plugins/code/plugin.min.js'; //编辑源码
import './plugins/codesample/plugin.min.js'; //代码示例
import './plugins/directionality/plugin.min.js'; //文字方向
import './plugins/emoticons/plugin.min.js'; //表情
import './plugins/fullscreen/plugin.min.js'; //全屏
import './plugins/help/plugin.min.js'; //帮助
import './plugins/image/plugin.min.js'; //插入编辑图片
import './plugins/importcss/plugin.min.js'; //引入css
import './plugins/insertdatetime/plugin.min.js'; //插入日期时间
import './plugins/link/plugin.min.js'; //超链接
import './plugins/lists/plugin.min.js'; //列表插件
import './plugins/media/plugin.min.js'; //插入编辑媒体
import './plugins/nonbreaking/plugin.min.js'; //插入不间断空格
import './plugins/pagebreak/plugin.min.js'; //插入分页符
import './plugins/preview/plugin.min.js'; //预览
import './plugins/quickbars/plugin.min.js'; //快速工具栏
import './plugins/save/plugin.min.js'; //保存
import './plugins/searchreplace/plugin.min.js'; //查找替换
import './plugins/table/plugin.min.js'; //表格
import './plugins/template/plugin.min.js'; //内容模板
import './plugins/visualblocks/plugin.min.js'; //显示元素范围
import './plugins/visualchars/plugin.min.js'; //显示不可见字符
import './plugins/wordcount/plugin.min.js'; //字数统计

const props = defineProps({
  modelValue: {
    type: String,
    required: true,
    default: '',
  },
  menubar: {
    type: [Boolean, String],
    default: 'file edit insert view format table tools help',
  },

  height: {
    type: Number,
    default: 400,
  },
  width: {
    type: Number,
    default: 600,
  },
  id: {
    type: [String, Number],
    default: 'myTinymce',
  },
});

const emit = defineEmits(['update:modelValue']);

let contentValue = computed({
  get() {
    return props.modelValue;
  },
  set(value) {
    emit('update:modelValue', value);
  },
});

//代码表格格式刷引用图片
onMounted(async () => {
  tinymce.init({
    selector: '#textarea',
    language: 'zh-Hans', //汉化
    width: props.width,
    height: props.height,
    branding: false,
    // ?
    contextmenu: "bold copy ",
    font_family_formats:
      '微软雅黑=Microsoft YaHei,Helvetica Neue,PingFang SC,sans-serif;苹果苹方=PingFang SC,Microsoft YaHei,sans-serif;宋体=simsun,serif;仿宋体=FangSong,serif;黑体=SimHei,sans-serif;Arial=arial,helvetica,sans-serif;Arial Black=arial black,avant garde;Book Antiqua=book antiqua,palatino;',
    font_size_formats: '12px 14px 16px 18px 20px 22px 24px 28px 32px 36px 48px 56px 72px', //字体大小
    icons_url: './icons/default/icons.min.js', // load icon pack
    icons: 'custom',      // baseURL/icons/custom/icons.js
    // ?
    // menubar: 'file edit insert view format table tools help',
    // menu: {
    //   file: { title: '文件', items: 'newdocument' },
    //   edit: { title: '编辑', items: 'undo redo | cut copy paste pastetext | selectall' },
    //   insert: { title: '插入', items: 'link media | template hr' },
    //   view: { title: '查看', items: 'visualaid' },
    //   format: { title: '格式', items: 'bold italic underline strikethrough superscript subscript | formats | removeformat' },
    //   table: { title: '表格', items: 'inserttable tableprops deletetable | cell row column' },
    //   tools: { title: '工具', items: 'spellchecker code' }
    // },

    menubar: 'file edit view insert format tools table help',
    menu: {
      file: { title: 'File', items: 'newdocument | preview | export | deleteallconversations' },
      edit: { title: 'Edit', items: 'undo redo restoredraft | cut copy | selectall | searchreplace' },
      view: { title: 'View', items: 'code | visualaid visualchars visualblocks | preview fullscreen | showcomments' },
      insert: {
        title: 'Insert',
        items: 'image link media addcomment pageembed template codesample inserttable | charmap emoticons | pagebreak nonbreaking anchor tableofcontents | insertdatetime',
      },
      format: {
        title: 'Format',
        items: 'bold italic underline strikethrough superscript subscript codeformat | styles blocks fontfamily fontsize align lineheight | forecolor backcolor | language | removeformat',
      },
      tools: { title: 'Tools', items: 'a11ycheck code wordcount' },
      table: { title: 'Table', items: 'inserttable | cell row column | advtablesort | tableprops deletetable' },
      help: { title: 'Help', items: 'help' },
    },
    style_formats: [
      { title: 'Bold text', inline: 'b' },
      { title: 'Red text', inline: 'span', styles: { color: '#ff0000' } },
      { title: 'Red header', block: 'h1', styles: { color: '#ff0000' } },
      { title: 'Example 1', inline: 'span', classes: 'example1' },
      { title: 'Example 2', inline: 'span', classes: 'example2' },
      { title: '首行缩进', block: 'p', styles: { 'text-indent': '2em' } },
      { title: 'Table styles' },
      { title: 'Table row 1', selector: 'tr', classes: 'tablerow1' }
    ],
    style_formats_merge: true,
    style_formats_autohide: true,
    //

    color_cols: 7,
    custom_colors: false,
    color_map: [
      "000000", "Black",
      "993300", "Burnt orange",
      "333300", "Dark olive",
      "003300", "Dark green",
      "003366", "Dark azure",
      "000080", "Navy Blue",
      "333399", "Indigo",
      "333333", "Very dark gray",
      "800000", "Maroon",
      "ec6842", "imperial orange",
      "4C963E", "light green",
      "fdd700", "moor yellow",
      "2837ae", "deep sea",
    ],

    toolbar:
      'fullscreen | code forecolor backcolor bold italic underline strikethrough link anchor | alignleft aligncenter alignright alignjustify outdent indent  lineheight | styleselect formatselect fontselect fontsizeselect | bullist numlist | blockquote subscript superscript removeformat | table image media | indent2em formatpainter axupimgs',
    toolbar_mode: 'sliding',
    plugins:
      'code codesample preview searchreplace autolink directionality visualblocks visualchars fullscreen image link media template table charmap pagebreak nonbreaking anchor insertdatetime advlist lists wordcount autosave ',
    line_height_formats: '1 1.2 1.4 1.6 2', //行高

    allow_conditional_comments: true,
    formats: {
      code: { block: 'p', styles: { 'background-color': 'lightgray' } },
      blockquote: { block: 'p', styles: { 'border-left': '2px solid #ccc', 'margin-left': '1.5rem', 'padding-left': '1rem' } },
    },


    // images_file_types: 'jpeg,jpg,png,gif,bmp',
    // //images_upload_handler: editorUploadImage(blobInfo, 100),

    // elementpath: false, //元素路径是否显示
    custom_undo_redo_levels: 10, //撤销和重做的次数
    // draggable_modal: true, //对话框允许拖拽

  }); //初始化
});
onUnmounted(() => {
  tinymce.remove(); //销毁
});
</script>
<template>
  <div class="tinymce-box">
    <textarea id="textarea" v-model="contentValue" />
  </div>
</template>
<style scoped>
.tinymce-box {
  width: 100%;
}
</style>
