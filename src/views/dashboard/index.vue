<template>
  <div class="dashboard-container">
    <!-- <UEditor style="marginTop:10px;" ref="ueditor"></UEditor> -->

    <el-row  style="marginTop:10px;">
        <el-col :span="4"><div class="searchTitle">标题</div></el-col>
        <el-col :span="12"><el-input class="" v-model="changeOptions.title" placeholder="请输入内容"></el-input></el-col>
    </el-row>

    <el-row  style="marginTop:10px;">
        <el-col :span="4"><div class="searchTitle">作者：</div></el-col>
        <el-col :span="12"><el-input class="" v-model="changeOptions.author" placeholder="请输入内容"></el-input></el-col>
    </el-row>

    <wang-editor ref="wangUeitors"></wang-editor>
    <el-row>
      <el-button type="primary" @click="publicContent">发布文章</el-button>
    </el-row>
  </div>
</template>

<script>
import WangEditor from '@/components/WangEditor'
import { mapGetters } from 'vuex'
import { articlePublishApi } from '@/api/article'

export default {
  name: 'dashboard',
  data() {
    return {
      changeOptions: {
        title: '',
        author: '',
        content: ''
      }
    }
  },
  components: {
    WangEditor
  },
  computed: {
    ...mapGetters([
      'name',
      'roles'
    ])
  },
  methods: {
    publicContent() {
      this.changeOptions.content = this.$refs.wangUeitors.getwangUditorHtml();
      articlePublishApi(this.changeOptions).then( res => {
        if(res.data.code != 0) {
          this.$message.error(res.data.message);
        }else {
          this.$message({
            message: '发布成功',
            type: 'success'
          });
        }
      })
    }
  }
}
</script>

<style rel="stylesheet/scss" lang="scss" scoped>
.dashboard {
  &-container {
    margin: 30px;
  }
  &-text {
    font-size: 30px;
    line-height: 46px;
  }
}
</style>
