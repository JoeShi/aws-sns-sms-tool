<template>
  <section>
    <el-container>
      <el-header style="-webkit-app-region: drag">
        <h1 class="logo">
          <img src="../../../static/image/logo.png" alt="logo">
        </h1>
        <div class="quit" @click="quit">
          <img src="../../../static/image/quit.svg" alt="quit">
        </div>
        <AddImage class="addimg"></AddImage>
      </el-header>
      <el-container>
        <el-aside width="200px">
          <h4 class="title">存储空间列表
            <i class="el-icon-plus add" @click="addDialogVisible = true"></i>
          </h4>
          <el-menu default-active="0" class="el-menu-vertical-demo">
            <el-menu-item :index="''+index" v-for="(item,index) in options" :key="index" @click="value = item">
              <i class="icon"></i>
              <span slot="title">{{item}}</span>
              <i class="el-icon-delete delete" @click.stop.prevent="beforeDelete(item)"></i>
            </el-menu-item>
          </el-menu>
        </el-aside>
        <el-main>
          <div class="bucket" v-if="options.length" style="-webkit-app-region: no-drag">
            <List :bucket="value" :mac="mac" :url="url" :postData="postData" :action="action"></List>
          </div>
        </el-main>
      </el-container>
    </el-container>
    <el-dialog title="提示" :visible.sync="addDialogVisible" width="40%">
      <el-form ref="form" :model="form" label-width="100px">
        <el-form-item label="存储空间名称">
          <el-input v-model="form.name"></el-input>
        </el-form-item>
        <el-form-item label="存储区域">
          <el-select v-model="form.region" placeholder="请选择存储区域">
            <el-option label="华东" value="z0"></el-option>
            <el-option label="华北" value="z1"></el-option>
            <el-option label="华南" value="z2"></el-option>
            <el-option label="北美" value="na0"></el-option>
            <el-option label="东南亚" value="as0"></el-option>
          </el-select>
        </el-form-item>
      </el-form>
      <span slot="footer" class="dialog-footer">
        <el-button size="mini" @click="addDialogVisible = false">取 消</el-button>
        <el-button size="mini" type="primary" @click="addBucket">确 定</el-button>
      </span>
    </el-dialog>
  </section>
</template>

<script>
// import qiniu from 'qiniu'
import List from '@/components/Manage/List'
import Upload from '@/components/Upload'
import AddImage from '@/components/AddImage'

export default {
  data () {
    return {
      mac: {},
      options: [],
      value: '',
      url: '',
      img: '',
      postData: {
        token: ''
      },
      AccessToken: '',
      action: '',
      addDialogVisible: false,
      form: {
        name: '',
        region: ''
      }
    }
  },
  methods: {
    quit () {
      localStorage.removeItem('obj')
      this.$electron.ipcRenderer.send('status', false)
    }
  },
  mounted () {
  },
  components: {
    List,
    Upload,
    AddImage
  }
}
</script>

<style lang="scss" scoped>
.el-select {
  display: block;
}
.el-container {
  height: 650px;
  .el-header {
    background-color: #409eff;
    color: #fff;
    position: relative;
    .logo {
      width: 125px;
      height: 29px;
      margin-top: 22px;
      img {
        width: 100%;
        height: auto;
        vertical-align: top;
      }
    }
    .quit {
      position: absolute;
      bottom: 20px;
      right: 20px;
      width: 15px;
      img {
        width: 100%;
        height: auto;
      }
    }
    .addimg {
      position: absolute;
      left: 200px;
      bottom: 20px;
      font-size: 12px;
      line-height: 22px;
    }
  }
  .el-aside {
    border-right: solid 1px #ebeef5;
    background: #fff;
    .delete {
      position: absolute;
      right: 0;
      top: 12px;
      color: #f36d6e;
    }
    .title {
      font-weight: normal;
      font-size: 12px;
      padding: 10px;
      color: #909399;
      overflow: hidden;
      .add {
        float: right;
        background: #409eff;
        border-radius: 100%;
        padding: 2px;
        color: #fff;
        cursor: pointer;
      }
    }
    span {
      overflow: hidden;
      text-overflow: ellipsis;
      word-wrap: normal;
      display: inline-block;
      width: 130px;
    }
    .icon {
      background: url("../../../static/image/data.png") no-repeat;
      width: 18px;
      height: 18px;
      background-size: contain;
      display: inline-block;
      margin-right: 5px;
    }
    .el-menu {
      border: 0;
      .el-menu-item,
      .el-submenu__title {
        height: 40px;
        line-height: 40px;
        position: relative;
      }
      .is-active {
        background-color: #ecf5ff;
        position: relative;
        &::before {
          content: "";
          position: absolute;
          left: 0;
          top: 0;
          width: 4px;
          height: 100%;
          background: #409eff;
        }
      }
    }
  }
  .el-main {
    padding: 0;
  }
}
.bucket {
  .bucket-select {
    margin-bottom: 10px;
  }
  .bucket-img {
    white-space: nowrap;
    span {
      color: #ff3e3e;
    }
  }
  .copy {
    margin-left: 10px;
    border: 0;
    padding: 5px 10px;
    border-radius: 4px;
    background: #2c9cfb;
    color: #fff;
  }
}
</style>
