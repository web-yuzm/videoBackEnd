<template>
  <div class="topicCategoryList">
    <div class="topicCategoryList">
<el-table
    :data="tableData"
    border
    style="width: 100%">
     <el-table-column
      label="id"
      width="120">
      <template scope="scope">
        <span>{{ scope.row.id }}</span>
      </template>
    </el-table-column>
    <el-table-column
      label="类名"
      width="180">
      <template scope="scope">
          <div slot="reference" class="name-wrapper">
            <el-tag>{{ scope.row.name }}</el-tag>
          </div>
      </template>
    </el-table-column>
    <el-table-column label="操作">
      <template scope="scope">
        <el-button
          size="small"
          @click="topicCategoryEdit(scope.row.id)">编辑</el-button>
        <el-button
          size="small"
          type="danger"
          @click="topicCategoryDelete(scope.row.id, scope.$index, tableData)">删除</el-button>
        </template>
        </el-table-column>
     </el-table>
      </div>
    <div>
      <router-view></router-view>
    </div>
  </div>
</template>

<script>
import { LOCALHOST_URL } from '../assets/js/localhost.js'

  export default {
    data() {
      return {
        tableData: []
      }
    },
    created () {
        this.$store.dispatch('setTitlename', {name:'话题分类'})
        this.$http.get(''+LOCALHOST_URL+'/api/topicCategoryList').then((response)=>{
          let body = response.body;
          var data = [];
          let _this = this;
          for(let i=0;i<body.length;i++){
            var obj = {};
            obj.id = body[i].tid;
            obj.name = body[i].name;
            data[i] = obj;
          }
          _this.tableData = data;
        })
    },
    methods: {
      topicCategoryEdit(id) {
        this.$router.push('/topicCategoryEdit?id='+ id);
      },
      topicCategoryDelete(id, index,rows) {
        this.$confirm('此操作将删除该分类及其所属该分类的所有内容, 请慎重!', '提示', {
          confirmButtonText: '确定',
          cancelButtonText: '取消',
          type: 'warning'
        }).then(() => {
            this.$http.post(''+LOCALHOST_URL+'/api/topicCategoryDel',{
            id : id
          }).then((response) => {
              console.log(response)
            },(response)=>{
              console.log(response)
         });
            rows.splice(index, 1)
            this.$message({
                type: 'success',
                message: '删除成功!'
          });
        }).catch(() => {
          this.$message({
            type: 'info',
            message: '已取消删除'
          });          
        });
      }
    }
  }
</script>

<style>
</style>