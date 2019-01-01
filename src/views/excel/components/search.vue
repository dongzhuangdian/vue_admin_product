<template>
  <div>
    <div>
      <lable>文件类型</lable>
      <el-input
        placeholder="请输入内容"
        v-model="input10"
        clearable>
      </el-input>
      <el-button type="primary" icon="el-icon-search">搜索</el-button>
      <i style="float:right;margin-top:20px" class="el-icon-setting" @click="isShowCtl"></i>
    </div>
    <div  v-show="isShow" class="table_col_ctl">
      <table-col-ctl></table-col-ctl>
    </div>
  </div>

</template>

<script>
  import tableColCtl from "./tableColCtl.vue"
  export default {
    components:{tableColCtl},
    data() {
      return {
        input10: '',
        isShow:false
      }
    },
    methods:{
        isShowCtl :function(){
          this.isShow = this.isShow == false ? true : false
        }
    }
  }
</script>

<style lang="less">
  .el-input {
      width: 10%;
  }

  .table-col-ctl {
    background-color: white;
    position: fixed;
    left: 80%;
    top: 85px;
    z-index: 10;
  }
</style>
