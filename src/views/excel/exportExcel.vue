<template>
  <!-- $t is vue-i18n global function to translate lang -->
  <div class="app-container">

    <div>
      <BookTypeOption v-model="bookType" />
      <el-button :loading="downloadLoading" style="margin:0 0 20px 20px;" type="primary" icon="document" @click="handleDownload">Excel</el-button>
      <a href="https://panjiachen.github.io/vue-element-admin-site/feature/component/excel.html" target="_blank" style="margin-left:15px;">
        <el-tag type="info">Documentation</el-tag>
      </a>
    </div>

    <el-table v-loading="listLoading" :data="list" element-loading-text="拼命加载中" fit highlight-current-row>
      <el-table-column type="expand">
        <template slot-scope="props">
          <el-table>
            <el-table-column
              v-if="false"
              label="#"
              prop="id"/>
            <el-table-column
              v-if="false"
              label="文件"
              prop="fileName"/>
            <el-table-column
              v-if="false"
              label="最近修改时间"
              prop="lastModifyTime"/>
            <el-table-column
              v-if="false"
              label="责任人"
              prop="responsiblePerson"/>
            <el-table-column
              v-if="false"
              label="原因"
              prop="cause"/>
          </el-table>
          <!-- <el-form label-position="left" inline class="demo-table-expand"> -->
        </template>
      </el-table-column>
      <el-table-column
        label="#"
        prop="id"/>
      <el-table-column
        label="文件"
        prop="fileName"/>
      <el-table-column
        label="最近修改时间"
        prop="lastModifyTime"/>
      <el-table-column
        label="责任人"
        prop="responsiblePerson"/>
      <el-table-column
        label="原因"
        prop="cause"/>
      <el-table-column
        align="right">
        <template slot-scope="scope">
          <edit/>
        </template>
      </el-table-column>
    </el-table>
  </div>
</template>

<script>
import { fetchList } from '@/api/article'
import { parseTime } from '@/utils'
import edit from './components/Edit'

// options components
import BookTypeOption from './components/BookTypeOption'

export default {
  name: 'ExportExcel',
  components: { BookTypeOption, edit },
  data() {
    return {
      list: null,
      listLoading: true,
      downloadLoading: false,
      filename: '',
      autoWidth: true,
      bookType: 'xlsx'
    }
  },
  created() {
    this.fetchData()
  },
  methods: {
    fetchData() {
      this.listLoading = true
      console.log('test')
      fetchList().then(response => {
        this.list = response.items
        console.log(this.list)
        this.listLoading = false
      }).catch(v => {
        console.log('hello')
        console.log(v)
      })
    },
    handleDownload() {
      this.downloadLoading = true
      import('@/vendor/Export2Excel').then(excel => {
        const tHeader = ['Id', 'Title', 'Author', 'Readings', 'Date']
        const filterVal = ['id', 'title', 'author', 'pageviews', 'display_time']
        const list = this.list
        const data = this.formatJson(filterVal, list)
        excel.export_json_to_excel({
          header: tHeader,
          data,
          // filename: 'excel_' + Date.getFullYear() + '_' + Date.getMonth() + '_' + Date.getDate() + ,
          filename: 'excel_' + this.getNowTime(),
          autoWidth: this.autoWidth,
          bookType: this.bookType
        })
        this.downloadLoading = false
      })
    },

    getNowTime() {
      var date = new Date()
      var Y = date.getFullYear() + '-'
      var M = (date.getMonth() + 1 < 10 ? '0' + (date.getMonth() + 1) : date.getMonth() + 1) + '-'
      var D = date.getDate() + ' '
      var h = date.getHours() + ':'
      var m = date.getMinutes() + ':'
      var s = date.getSeconds()
      console.log(Y + M + D + h + m + s)
      return Y + M + D + h + m + s
    },
    formatJson(filterVal, jsonData) {
      return jsonData.map(v => filterVal.map(j => {
        if (j === 'timestamp') {
          return parseTime(v[j])
        } else {
          return v[j]
        }
      }))
    },

    editInfo() {
      alert('vue')
    }
  }
}
</script>

<style>
.radio-label {
  font-size: 14px ! important;
  color: #606266;
  line-height: 40px;
  padding: 0 0px 0 0px;
}
.el-table::before {
  height: 0px;
}
.el-table__empty-block {
  min-height: 10px;
}
</style>

