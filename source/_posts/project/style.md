项目中存在的问题
1. style没有打包成单独的文件，都是小的样式请求文件，增加了请求次数。
2. 各个组件的相同的element样式没有抽离，存在样式冗余。
3. 类名风格没有统一。BEM风格，参照element样式，设计组件时层次结构（语义化标签）

1. el-form-item
.el-form-item .is-success .is-error .is-required
  .el-form-item__content
    .el-input .is-disabled
      .el-input__inner disabled="disabled"
    // 
    .el-select
      .el-input
      .el-select-dropdown .el-popover
  .el-form-item__error

2. 