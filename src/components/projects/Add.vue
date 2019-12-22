<template>
  <div>
    <!-- 面包屑导航区域 -->
    <el-breadcrumb separator-class="el-icon-arrow-right">
      <el-breadcrumb-item :to="{ path: '/home' }">Dashboard</el-breadcrumb-item>
      <el-breadcrumb-item>Project Manage</el-breadcrumb-item>
      <el-breadcrumb-item>Add Project</el-breadcrumb-item>
    </el-breadcrumb>

    <!-- 卡片视图 -->
    <el-card>
      <!-- 提示区域 -->
      <el-alert title="Add Project Info" type="info" center show-icon :closable="false">
      </el-alert>
      <!-- 步骤条区域 -->
      <el-steps :space="200" :active="activeIndex - 0" finish-status="success" align-center>
        <el-step title="Base Info"></el-step>
        <el-step title="Task Group1"></el-step>
        <el-step title="Task Group2"></el-step>
        <el-step title="Task Group3"></el-step>
        <el-step title="Task Group4"></el-step>
        <el-step title="Complete"></el-step>
      </el-steps>

      <!-- tab栏区域 -->

      <el-form :model="addForm" :rules="addFormRules" ref="addFormRef" label-width="100px" label-position="top">
        <el-tabs v-model="activeIndex" :tab-position="'left'" :before-leave="beforeTabLeave" @tab-click="tabClicked">
          <el-tab-pane label="Base Info" name="0">
            <el-form-item label="Project" prop="goods_name">
              <el-input v-model="addForm.goods_name"></el-input>
            </el-form-item>
            <el-form-item label="Description" prop="goods_price">
              <el-input v-model="addForm.goods_price"></el-input>
            </el-form-item>
          </el-tab-pane>
          <el-tab-pane label="Task Group1" name="1">
            <!-- 渲染表单的Item项 -->
            <el-form-item :label="item.attr_name" v-for="item in manyTableData" :key="item.attr_id">
              <!-- 复选框组 -->
              <el-checkbox-group v-model="item.attr_vals">
                <el-checkbox :label="cb" v-for="(cb, i) in item.attr_vals" :key="i" border></el-checkbox>
              </el-checkbox-group>
              <template>
                <el-transfer v-model="value" :data="data"></el-transfer>
              </template>
            </el-form-item>
          </el-tab-pane>
          <el-tab-pane label="Task Group2" name="2">
            <el-form-item :label="item.attr_name" v-for="item in onlyTableData" :key="item.attr_id">
              <el-input v-model="item.attr_vals"></el-input>
            </el-form-item>
            <template>
              <el-transfer v-model="value" :data="data"></el-transfer>
            </template>
          </el-tab-pane>
          <el-tab-pane label="Task Group3" name="3">
            Select Tasks:
            <el-row>
              <el-col :offset="3">
                <template>
                  <el-transfer v-model="value" :data="data"></el-transfer>
                </template>
              </el-col>

            </el-row>
            <el-row>Customed Tasks:
              <el-table :data="taskslist" border stripe>
                <el-table-column label="Task" prop="taskname"></el-table-column>
                <el-table-column label="Project" prop="projectname"></el-table-column>
                <el-table-column
                  prop="groupname"
                  label="Group"
                  width="100"
                  :filters="[{ text: 'Group 001', value: 'Group 001' }, { text: 'Group 002', value: 'Group 002' }]"
                  :filter-method="filterTag"
                  filter-placement="bottom-end">
                  <template slot-scope="scope">
                    <el-tag
                      :type="scope.row.groupname === 'Group 001' ? 'primary' : 'success'"
                      disable-transitions>{{scope.row.groupname}}</el-tag>
                  </template>
                </el-table-column>
                <el-table-column label="Executor" prop="comment"></el-table-column>

                <el-table-column label="Option" width="180px">
                  <template slot-scope="scope">
                    <!-- 完成 -->
                    <el-button type="info" icon="el-icon-check" circle></el-button>
                    <!-- 提醒 -->
                    <el-button type="danger" icon="el-icon-message" circle></el-button>
                  </template>
                </el-table-column>
              </el-table>

            </el-row>


          </el-tab-pane>
          <el-tab-pane label="Task Group4" name="4">select tasks</el-tab-pane>
        </el-tabs>
      </el-form>

    </el-card>
  </div>
</template>

<script>
export default {
  data() {
    return {
      activeIndex: '0',
      // 添加商品的表单数据对象
      addForm: {
        goods_name: '',
        goods_price: 0,
        goods_weight: 0,
        goods_number: 0,
        // 商品所属的分类数组
        goods_cat: []
      },
      addFormRules: {
        goods_name: [
          { required: true, message: '请输入商品名称', trigger: 'blur' }
        ],
        goods_price: [
          { required: true, message: '请输入商品价格', trigger: 'blur' }
        ]
      },
      // 商品分类列表
      catelist: [],
      cateProps: {
        label: 'cat_name',
        value: 'cat_id',
        children: 'children'
      },
      // 动态参数列表数据
      manyTableData: [],
      // 静态属性列表数据
      onlyTableData: []
    }
  },
  created() {
    this.getCateList()
  },
  methods: {
    // 获取所有商品分类数据
    async getCateList() {
      const { data: res } = await this.$http.get('categories')

      if (res.meta.status !== 200) {
        return this.$message.error('获取商品分类数据失败！')
      }

      this.catelist = res.data
      console.log(this.catelist)
    },
    // 级联选择器选中项变化，会触发这个函数
    handleChange() {
      console.log(this.addForm.goods_cat)
      if (this.addForm.goods_cat.length !== 3) {
        this.addForm.goods_cat = []
      }
    },
    beforeTabLeave(activeName, oldActiveName) {
      // console.log('即将离开的标签页名字是：' + oldActiveName)
      // console.log('即将进入的标签页名字是：' + activeName)
      // return false
      // if (oldActiveName === '0' && this.addForm.goods_cat.length !== 3) {
      //   this.$message.error('请先选择商品分类！')
      //   return false
      // }
    },
    async tabClicked() {
      // console.log(this.activeIndex)
      // 证明访问的是动态参数面板
      if (this.activeIndex === '1') {
        const { data: res } = await this.$http.get(
          `categories/${this.cateId}/attributes`,
          {
            params: { sel: 'many' }
          }
        )

        if (res.meta.status !== 200) {
          return this.$message.error('获取动态参数列表失败！')
        }

        console.log(res.data)
        res.data.forEach(item => {
          item.attr_vals =
            item.attr_vals.length === 0 ? [] : item.attr_vals.split(' ')
        })
        this.manyTableData = res.data
      } else if (this.activeIndex === '2') {
        const { data: res } = await this.$http.get(
          `categories/${this.cateId}/attributes`,
          {
            params: { sel: 'only' }
          }
        )

        if (res.meta.status !== 200) {
          return this.$message.error('获取静态属性失败！')
        }

        console.log(res.data)
        this.onlyTableData = res.data
      }
    }
  },
  computed: {
    cateId() {
      if (this.addForm.goods_cat.length === 3) {
        return this.addForm.goods_cat[2]
      }
      return null
    }
  }
}
</script>

<style lang="less" scoped>
.el-checkbox {
  margin: 0 10px 0 0 !important;
}
</style>
