<template>
  <div class="dashboard-container">
    <el-table
    :data="tableData"
    :border="true"
    mpty-text="暂无数据"
    style="width: 100%">
        <el-table-column
        label="ID"
        align="center"
        width="180">
            <template slot-scope="scope">
                <div>{{scope.row.id}}</div>
            </template>
        </el-table-column>
        <el-table-column
        label="标题"
        align="center"
        width="180">
        <template slot-scope="scope">
            <div>{{scope.row.title}}</div>
        </template>
        </el-table-column>
        <el-table-column
        label="作者"
        align="center"
        width="180">
        <template slot-scope="scope">
            <div>{{scope.row.author}}</div>
        </template>
        </el-table-column>
        <el-table-column
        label="是否推荐"
        align="center"
        width="180">
            <template slot-scope="scope">
                <div v-if="scope.row.recommend == '0'">是</div>
                <div v-else>否</div>
            </template>
        </el-table-column>
        <el-table-column 
        align="center"
        label="操作">
            <template slot-scope="scope">
                <el-button
                size="mini"
                @click="handleEdit(scope.$index, scope.row)">编辑</el-button>
                <el-button
                size="mini"
                type="danger"
                @click="handleDelete(scope.$index, scope.row)">删除</el-button>
            </template>
        </el-table-column>
    </el-table>

    <!-- 修改和添加弹出框 -->
    <el-dialog
    title="提示"
    :visible.sync="centerDialogVisible"
    width="90%"
    center>
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
        <span slot="footer" class="dialog-footer">
            <el-button @click="centerDialogVisible = false">取 消</el-button>
            <el-button type="primary" @click="centerDialogVisible = false">确 定</el-button>
        </span>
    </el-dialog>
  </div>
</template>

<script>
import WangEditor from '@/components/WangEditor'
import { articlePublishApi, articleApi, articleListApi } from '@/api/article'

export default {
  name: 'dashboard',
  data() {
    return {
      tableData: [],
      centerDialogVisible: false,
      changeOptions: {
        title: '',
        author: '',
        content: ''
      },
    }
  },
  components: {
    WangEditor
  },
  mounted() {
      articleListApi().then(res => {
          this.tableData = res.data.data;
      })
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
        //   articleListApi().then(response => {
        //     console.log('====>',response.data.data.length-1)
        //     console.log(response.data.data[response.data.data.length-1].id);
        //     articleApi({id: response.data.data[response.data.data.length-1].id}).then( resp => {
        //       this.html = resp.data.data.content;
        //     })
        //   })
        }
      })
    },
    handleEdit(index, row) {
        console.log(index, row);
        this.centerDialogVisible = true;
    },
    handleDelete(index, row) {
        console.log(index, row);
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
