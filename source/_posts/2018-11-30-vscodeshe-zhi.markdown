---
layout: post
title: "Vscode设置"
date: 2018-11-30 14:22:40 +0800
comments: true
categories: vscode
---

## vscode开发工具设置
* 快捷键 `command + ,` 打开设置，右上角打开 *settings.json*
<!-- more -->
```json
{
  "workbench.iconTheme": "material-icon-theme",
  "materialTheme.fixIconsRunning": false,
  "editor.fontSize": 16, // 字体大小
  // 以下是代码格式化配置
  "editor.formatOnSave": true, // 每次保存的时候自动格式化
  "editor.tabSize": 2, // 代码缩进修改成2个空格
  "eslint.autoFixOnSave": false, // 每次保存的时候将代码按eslint格式进行修复
  "prettier.eslintIntegration": true, // 让prettier使用eslint的代码格式进行校验
  "prettier.semi": false, // 去掉代码结尾的分号
  "prettier.singleQuote": true, // 使用带引号替代双引号
  "javascript.format.insertSpaceBeforeFunctionParenthesis": true, // 让函数(名)和后面的括号之间加个空格
  "vetur.format.defaultFormatter.html": "js-beautify-html", //格式化.vue中html
  "vetur.format.defaultFormatter.js": "vscode-typescript", //让vue中的js按编辑器自带的ts格式进行格式化
  "vetur.format.defaultFormatterOptions": {
    "js-beautify-html": {
      "wrap_attributes": "force-aligned" //属性强制折行对齐
    }
  },
  "eslint.validate": [
    //开启对.vue文件中错误的检查
    "javascript",
    "javascriptreact",
    {
      "language": "html",
      "autoFix": true
    },
    {
      "language": "vue",
      "autoFix": true
    }
  ],
  "window.zoomLevel": 0,
  "editor.snippetSuggestions": "top",
  "editor.formatOnPaste": true,
  "explorer.confirmDelete": false,
  "todo-tree.defaultHighlight": {
    "foreground": "green",
    "type": "none"
  },
  "todo-tree.customHighlight": {
    "TODO": {},
    "FIXME": {}
  },
  "git.autofetch": true,
  "terminal.integrated.fontFamily": "'Meslo LG M DZ for Powerline'" //终端样式
}
```

* 打开工具栏 *Code -> 用户代码片段*, 新件vue模板, *vue.json*, 在vue文件中，键入`vue`，按下`tab`键快速生成模板
```json
{
  "Print to console": {
    "prefix": "vue",
    "body": [
      "<template>",
      "  <div class=\"wrapper\">$0</div>",
      "</template>",
      "",
      "<script>",
      "export default {",
      "  components: {},",
      "  props: {},",
      "  data () {",
      "    return {",
      "    }",
      "  },",
      "  watch: {},",
      "  computed: {},",
      "  methods: {},",
      "  created () { },",
      "  mounted () { }",
      "}",
      "</script>",
      "<style lang=\"scss\" scoped>",
      ".wrapper {",
      "}",
      "</style>",
      "$2"
    ],
    "description": "A vue file template"
  }
}
```

## vscode快捷键使用
* `command+K command+Q` 快速定位最后一次文件修改的地方
* `command+K command+S` 快捷键映射列表
* `command+P` 转到文件
* ctrl + ` 打开终端
* `command+shift+O` 转到文件中的符号
* `command+B` 隐藏或显示侧边栏
* `command+shift+剪头右` 选择光标后的文字