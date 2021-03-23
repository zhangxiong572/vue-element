<template>
  <div class="app-container">

    <el-form ref="form" :model="form" label-width="80px">
  <el-form-item label="活动名称">
    <el-input v-model="form.name" class="inp"></el-input>
  </el-form-item>
  <el-form-item label="活动区域">
    <el-select v-model="form.region" placeholder="请选择活动区域">
      <el-option label="区域一" value="shanghai"></el-option>
      <el-option label="区域二" value="beijing"></el-option>
    </el-select>
  </el-form-item>
<el-form-item label="活动区域">
    <el-select v-model="form.region" placeholder="请选择活动区域">
      <el-option label="区域一" value="shanghai"></el-option>
      <el-option label="区域二" value="beijing"></el-option>
    </el-select>
  </el-form-item>
 

  
</el-form>

  <el-row >
      <el-button type="primary" icon="el-icon-search" class="btn">搜搜</el-button>
  <el-button type="primary">新增</el-button>
</el-row>


    <el-table
      v-loading="listLoading"
      :data="list"
      element-loading-text="Loading"
      border
      fit
      highlight-current-row
      class="table"
    >
      <el-table-column align="center" label="序号" width="95">
        <template slot-scope="scope">
          {{ scope.$index + 1 }}
        </template>
</el-table-column>

<el-table-column align="center" prop="created_at" label="ID" width="200">
    <template slot-scope="scope">
          <i class="" />
          <span>{{ scope.row.id }}</span>
        </template>
</el-table-column>

<el-table-column label="姓名">
    <template slot-scope="scope">
          {{ scope.row.name }}
        </template>
</el-table-column>
<el-table-column label="地址" width="110" align="center">
    <template slot-scope="scope">
          <span>{{ scope.row.address }}</span>
        </template>
</el-table-column>
<el-table-column label="年龄" width="110" align="center">
    <template slot-scope="scope">
          {{ scope.row.age }}
        </template>
</el-table-column>
<el-table-column class-name="status-col" label="性别" width="110" align="center">
    <template slot-scope="scope">
          <el-tag :type="scope.row.status | statusFilter">{{ scope.row.sex }}</el-tag>
        </template>
</el-table-column>

<el-table-column label="操作">
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

<el-pagination @size-change="handleSizeChange" @current-change="handleCurrentChange" :current-page="currentPage" :page-sizes="[5, 10, 15, 20]" :page-size="pageSize" layout="total, sizes, prev, pager, next, jumper" :total="total">
</el-pagination>
</div>
</template>

<script>
    import {
        getList
    } from '@/api/table'
    import axios from 'axios'
    export default {
        filters: {
            statusFilter(status) {
                const statusMap = {
                    published: 'success',
                    draft: 'gray',
                    deleted: 'danger'
                }
                return statusMap[status]
            }
        },
        data() {
            return {
                currentPage: 1,
                total: 0,
                pageSize: 5,
                list: null,
                listLoading: true,
                form: {
                    name: '',
                    region: '',
                    date1: '',
                    date2: '',
                    delivery: false,
                    type: [],
                    resource: '',
                    desc: ''
                }
            }
        },
        created() {
            this.fetchData()
        },
        mounted() {
            this.getData()
        },
        methods: {
            fetchData() {
                this.listLoading = true
                getList().then(response => {
                    // this.list = response.data.items
                    this.listLoading = false
                })
            },
            getData() {
                axios.get('https://mock.mengxuegu.com/mock/60587df50d58b864da03d4e6/vuedemo/userdata').then(res => {
                    console.log(res.data.data)
                    let data = res.data.data
                    this.total = data.length
                        // data = data.slice(0, )
                    this.list = data

                })
            },
            handleDelete(index, row) {
                console.log(index, row);

                this.$confirm('确定删除' + row.name + '吗？', '提示', {
                    confirmButtonText: '确定',
                    cancelButtonText: '取消',
                    type: 'warning'
                }).then(() => {
                    console.log(this.list.length)

                    this.list.splice(index, 1)
                    this.$message({
                        type: 'success',
                        message: '删除成功'
                    })
                }).catch(() => {
                    this.$message({
                        type: 'info',
                        message: '已取消删除'
                    })
                })
            },
            handleSizeChange(val) {
                console.log(`每页 ${val} 条`);
                this.pageSize = val
                this.getData()
            },
            handleCurrentChange(val) {
                console.log(`当前页: ${val}`);
                this.currentPage = val
                this.getData()

            }
        }
    }
</script>

<style scoped>
    .inp {
        width: 200px;
    }
    
    .item {
        float: left;
    }
    
    .btn {
        margin-left: 30px;
    }
    
    .el-row {
        margin-left: 50px;
    }
    
    .table {
        margin-top: 20px;
    }
</style>