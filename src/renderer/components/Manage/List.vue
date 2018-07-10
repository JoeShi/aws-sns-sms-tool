<template>
  <div class="privileges-table">
    <el-table :data="filterTableData" style="width: 100%" v-loading="loading" max-height="590" size="small">
      <el-table-column prop="key" label="文件名" sortable>
      </el-table-column>
      <el-table-column prop="fsize" label="文件大小" sortable>
      </el-table-column>
      <el-table-column label="存储类型">
        <template slot-scope="scope">
          <i class="el-icon-document"></i>
          <span style="margin-left: 5px">{{ scope.row.mimeType }}</span>
        </template>
      </el-table-column>
      <el-table-column label="最后更新" width="160">
        <template slot-scope="scope">
          <i class="el-icon-time"></i>
          <span style="margin-left: 5px">{{ scope.row.putTime }}</span>
        </template>
      </el-table-column>
      <el-table-column label="操作" width="205">
        <template slot-scope="scope">
          <el-button size="mini" type="success" icon="el-icon-zoom-in" @click="preview(scope.$index, scope.row)"></el-button>
          <el-button size="mini" type="primary" @click="handleEdit(scope.$index, scope.row)">复制</el-button>
          <el-button size="mini" type="danger" @click="handleDelete(scope.$index, scope.row)">删除</el-button>
        </template>
      </el-table-column>
    </el-table>
    <el-dialog :title="title" :visible.sync="dialogFormVisible" @close="close">
      <Dialog :url="url" :postData="postData" :action="action" @onData="updateData"></Dialog>
    </el-dialog>
    <el-dialog title="预览" :visible.sync="PreviewDialogVisible" width="75%" class="previewimg">
      <img :src="previewUrl" alt="previewimg">
      <a :href="previewUrl" download="previewUrl" ref="img"></a>
      <span slot="footer" class="dialog-footer">
        <el-button size="mini" @click="copy">复制</el-button>
        <el-button size="mini" type="primary" @click="download">下载</el-button>
      </span>
    </el-dialog>
  </div>
</template>

<script>
import Dialog from '@/components/Manage/Dialog'
import Pagination from '@/components/Manage/Pagination'
import { mapState } from 'vuex'
export default {
  props: {
    bucket: {
      type: String
    },
    mac: {
      type: Object
    },
    url: {
      type: String
    },
    action: {
      type: String
    },
    postData: {
      type: Object
    }
  },
  data () {
    return {
      tableData: [],
      loading: false,
      rowList: {},
      dialogFormVisible: false,
      PreviewDialogVisible: false,
      currIndex: null,
      title: '',
      addStatus: false,
      search: '',
      AccessToken: '', // 存储空间列表 Authorization
      deleteAccessToken: '', // 删除资源 Authorization
      EncodedEntryURI: '', // 删除资源
      previewUrl: ''
    }
  },
  methods: {
    close () {
      this.$store.commit({
        type: 'ADD_IMAGE',
        data: false
      })
    }
  },
  computed: {
    ...mapState(['visible']),
    filterTableData () {
      if (this.search.length > 0) {
        return this.tableData.filter(it => {
          return it.key.indexOf(this.search) !== -1
        })
      } else {
        return this.tableData
      }
    }
  },
  mounted () {
  },
  watch: {
  },
  components: {
    Dialog,
    Pagination
  }
}
</script>

<style lang='scss' scoped>
.privileges-table {
  .table-hd {
    padding: 15px 0;
  }
  .pagination {
    background-color: #fff;
    padding: 15px 0;
    text-align: right;
  }
  .previewimg {
    text-align: center;
    img {
      max-width: 600px;
      max-height: 300px;
    }
  }
}
</style>
