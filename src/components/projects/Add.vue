<template>
  <div>
    <el-form :model="forms" ref="forms" :rules="rules">
      <el-table
        :data="tableData5"
        :row-key="getRowKeys"
        :expand-row-keys="expands"
        style="width: 100%">
        <el-table-column type="expand">  //type="expand" 带下层数据的字段
          <template scope="scope">
            <el-table class="demo-table-expand"
                      :data="scope.row.tableData3"
                      border
                      style="width: 100%">
              <el-table-column
                prop="tableData3.date"
                label="编号"
              >
              </el-table-column>
              <el-table-column
                prop="name"
                label="房间号"
              >
              </el-table-column>
              <el-table-column
                prop="name"
                label="房型">
              </el-table-column>
              <el-table-column
                prop="date"
                label="性别"
              >
              </el-table-column>
              <el-table-column
                prop="name"
                label="总床位"
              >
              </el-table-column>
              <el-table-column
                prop="address"
                label="状态">
              </el-table-column>
              <el-table-column
                prop="date"
                label="备注"
              >
              </el-table-column>
              <el-table-column align="center" label="操作" width="150">
                <template scope="scope">
                  <el-button size="small" type="success" @click="handleUpdate(scope.row)">修改
                  </el-button>
                  <el-button size="small" type="danger" @click="sure">删除
                  </el-button>
                </template>
              </el-table-column>
            </el-table>
          </template>
        </el-table-column>
        <el-table-column align="center"
                         label="楼宇"
                         prop="names">
        </el-table-column>
        <el-table-column align="center"
                         label="描述"
                         prop="desc">
        </el-table-column>
        <el-table-column align="center" label="操作" width="150">
          <template scope="scope">
            <el-button size="small" type="success" @click="handleUpdate(scope.row)">修改
            </el-button>
            <el-button size="small" type="danger" @click="sure">删除
            </el-button>
          </template>
        </el-table-column>
      </el-table>
    </el-form>
    <el-button type="primary" @click="save">批量开票</el-button>
  </div>
</template>

<script>
  export default {
    name: "table",
    data(){
      return {
        forms:{
          id:1,
          documentNo:null,
          buyerName:"服务技术",
          buyerDp:"42118XXXXXXXXXX72X",
          buyerBankAccount:"招商银行4890284",
          buyerAddressPhone:"深圳市宝安区110112",
          buyerEmail:null,
          buyerPhone:null,
          buyerType:"01",
          remarks:"这是备注",
          total:350.9,
          voList:[
            {
              goodsName:"黄金",
              goodsCode:"44021940",
              specification:null,
              unit:"克",
              num:1,
              unitPrice:291.37,
              taxRate:0.17,
              taxAmount:49.53,
              favouredPolicy:"0",
              zeroTaxRate:"",
              hsbz:"1"
            },
            {
              goodsName:"花生",
              goodsCode:"4574511",
              specification:null,
              unit:"斤",
              num:1,
              unitPrice:8.55,
              taxRate:0.17,
              taxAmount:1.45,
              favouredPolicy:"0",
              zeroTaxRate:"",
              hsbz:"1"
            }
          ]
        },
        rules:{
          num:[{required:true,message:'数量不能为空',trigger:'blur'}],
          unitPrice:[{required:true,message:'单价不能为空',trigger:'blur'}]
        },
        tableData5: [{
          names: 'S1-科技楼',
          desc: '公司楼',
          tableData3: [{
            date: '2016-05-03',
            name: '王小虎',
            address: '上海市普陀区金沙江路 1518 弄'
          }]
        },
          {
            names: 'S2-科技楼',
            desc: '上课喽',
            tableData3: [{
              date: '2016-05-03',
              name: '张小虎',
              address: '上海市普陀区金沙江路 32421弄'
            }]
          }]
      }
    },
    methods:{
      save(){
        this.$refs['forms'].validate((valid)=>{
          if(valid){
            console.log(1)
          }
        })
      }
    }
  }
</script>
<style scoped lang="scss">

</style>
