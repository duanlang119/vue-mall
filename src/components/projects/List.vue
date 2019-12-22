<template>
  <div>
    <!-- 面包屑导航区域 -->
    <el-breadcrumb separator-class="el-icon-arrow-right">
      <el-breadcrumb-item :to="{ path: '/home' }">Dashboard</el-breadcrumb-item>
      <el-breadcrumb-item>Project Manage</el-breadcrumb-item>
      <el-breadcrumb-item>Project List</el-breadcrumb-item>
    </el-breadcrumb>

    <!-- 卡片视图区域 -->
    <el-card>
      <el-row :gutter="20">
        <el-col :span="8">
          <el-input placeholder="Search Project" v-model="queryInfo.query" clearable @clear="getProjectsList">
            <el-button slot="append" icon="el-icon-search" @click="getProjectsList"></el-button>
          </el-input>
        </el-col>
        <el-col :span="4">
          <el-button type="primary" @click="goAddpage">Add Project</el-button>
        </el-col>
      </el-row>

      <!-- table表格区域 -->
      <el-table :data="projectslist" border stripe>
        <el-table-column type="index"></el-table-column>
        <el-table-column label="Project" prop="project_name" width="95px"></el-table-column>
        <el-table-column label="Schedule" prop="project_process">
          <template slot-scope="scope">
          <el-progress :text-inside="true" :stroke-width="22" :percentage=getProcess(scope.row.project_process) status="exception"></el-progress>
            </template>
        </el-table-column>
        <el-table-column label="Expiration Time" prop="expire_time" width="140px">
          <template slot-scope="scope">
            {{scope.row.expire_time | dateFormat}}
          </template>
        </el-table-column>
        <el-table-column label="Create Time" prop="add_time" width="140px">
          <template slot-scope="scope">
            {{scope.row.add_time | dateFormat}}
          </template>
        </el-table-column>
        <el-table-column label="operation" width="130px">
          <template slot-scope="scope">
            <el-button type="primary" icon="el-icon-edit" size="mini"></el-button>
            <el-button type="danger" icon="el-icon-delete" size="mini" @click="removeById(scope.row.project_id)"></el-button>
          </template>
        </el-table-column>
      </el-table>

      <!-- 分页区域 -->
      <el-pagination @size-change="handleSizeChange" @current-change="handleCurrentChange" :current-page="queryInfo.pagenum" :page-sizes="[5, 10, 15, 20]" :page-size="queryInfo.pagesize" layout="total, sizes, prev, pager, next, jumper" :total="total" background>
      </el-pagination>
    </el-card>
  </div>
</template>

<script>
export default {
  data() {
    return {
      // 查询参数对象
      queryInfo: {
        query: '',
        pagenum: 1,
        pagesize: 10
      },
      // 商品列表
      projectslist: [],
      // 总数据条数
      total: 0
    }
  },
  created() {
    this.getProjectsList()
  },
  methods: {
    // 根据分页获取对应的商品列表
    async getProjectsList() {
      // const { data: res } = await this.$http.get('goods', {
      //   params: this.queryInfo
      // })
      //
      // if (res.meta.status !== 200) {
      //   return this.$message.error('获取商品列表失败！')
      // }
      //
      // this.$message.success('获取商品列表成功！')
      // console.log(res.data)
      // this.goodslist = res.data.goods
      // this.total = res.data.total
      const res_data={
        "total": 50,
        "pagenum": "1",
        "projects": [
          {
            "project_id": 144,
            "project_name": "asfdsd",
            "project_process": 80,
            "add_time": 1512954923,
            "expire_time": 1512954923
          }
        ]
      }
      this.projectslist = res_data.projects
      this.total = res_data.total
    },
    handleSizeChange(newSize) {
      this.queryInfo.pagesize = newSize
      this.getProjectsList()
    },
    handleCurrentChange(newPage) {
      this.queryInfo.pagenum = newPage
      this.getProjectsList()
    },
    async removeById(id) {
      const confirmResult = await this.$confirm(
        '此操作将永久删除该商品, 是否继续?',
        '提示',
        {
          confirmButtonText: '确定',
          cancelButtonText: '取消',
          type: 'warning'
        }
      ).catch(err => err)

      if (confirmResult !== 'confirm') {
        return this.$message.info('已经取消删除！')
      }

      const { data: res } = await this.$http.delete(`goods/${id}`)

      if (res.meta.status !== 200) {
        return this.$message.error('删除失败！')
      }

      this.$message.success('删除成功！')
      this.getProjectsList()
    },
    goAddpage() {
      this.$router.push('/projects/add')
    },
    getProcess(x) {
      return x
    }
  }
}
</script>

<style lang="less" scoped>
</style>
