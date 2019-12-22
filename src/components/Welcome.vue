<template>
  <div class="iCountUp">
    <el-card>
    <el-row :gutter="24">
      <el-col :span="6">
        <div class="wrap">
          <div class="grid-content bg-purple">
            <ICountUp :endVal="endVal" :duration="5000" @ready="onReady" />
            <div class="text">Total Visitors</div>
          </div>
          <i class="el-icon-s-custom"></i>
        </div>
      </el-col>

      <el-col :span="6">
        <div class="wrap">
          <div class="grid-content bg-purple">
            <ICountUp :endVal="endVal1" :duration="3000" @ready="onReady" />
            <div class="text">Messages</div>
          </div>
          <i class="el-icon-chat-dot-square"></i>
        </div>
      </el-col>

      <el-col :span="6">
        <div class="wrap">
          <div class="grid-content bg-purple">
            <ICountUp :endVal="endVal2" :duration="2000" @ready="onReady" />
            <div class="text">Total Tasks Placeed</div>
          </div>
          <i class="el-icon-shopping-cart-2"></i>
        </div>
      </el-col>

      <el-col :span="6">
        <div class="wrap">
          <div class="grid-content bg-purple">
            <ICountUp :endVal="endVal3" :duration="6000" @ready="onReady" />
            <div class="text">Total Projects</div>
          </div>
          <i class="el-icon-wallet"></i>
        </div>
      </el-col>
    </el-row>
      <!-- table表格区域 -->
      <el-table :data="projectslist" stripe>
        <el-table-column label="Project" prop="project_name" width="95px"></el-table-column>
        <el-table-column label="Expiration Time" prop="expire_time" width="140px">
          <template slot-scope="scope">
            {{scope.row.expire_time | dateFormat}}
          </template>
        </el-table-column>
        <el-table-column label="Schedule" prop="project_process">
          <template slot-scope="scope">
            <el-progress :text-inside="true" :stroke-width="22" :percentage=getProcess(scope.row.project_process) :status=getProcess(scope.row.project_status)></el-progress>
          </template>
        </el-table-column>
      </el-table>
      <div>
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
      </div>

    </el-card>
  </div>
</template>

<script>
  import ICountUp from "vue-count-to";
  export default {
    name: "demo",
    components: {
      ICountUp
    },
    data() {
      return {
        endVal: 67810,
        endVal1: 330,
        endVal2: 492,
        endVal3: 85045,
        projectslist:[{
          "project_id": 144,
          "project_name": "project 1",
          "project_process": 70,
          "project_status": "warning",
          "add_time": 1512954923,
          "expire_time": 1512954923
        },{
          "project_id": 145,
          "project_name": "project2",
          "project_process": 100,
          "project_status": "success",
          "add_time": 1512954923,
          "expire_time": 1512954923
        },{
          "project_id": 146,
          "project_name": "project3",
          "project_process": 80,
          "project_status": "warning",
          "add_time": 1512954923,
          "expire_time": 1512954923
        },{
          "project_id": 147,
          "project_name": "project4",
          "project_process": 20,
          "project_status": "exception",
          "add_time": 1512954923,
          "expire_time": 1512954923
        }],
        taskslist:[{"id": 25,
          "taskname": "tige117",
          "groupname": "Group 001",
          "projectname": "project3",
          "comment": "tige112@163.com",
          "create_time": "2017-11-09T20:36:26.000Z",
        },
          {"id": 26,
            "taskname": "abcd333",
            "groupname": "Group 002",
            "projectname": "project1",
            "comment": "tige112@164.com",
            "create_time": "2017-11-09T20:36:26.000Z",
          }]
      };
    },
    methods: {
      onReady: function(instance, CountUp) {
        const that = this;
        instance.update(that.endVal + 100);
      },
      getProcess(x) {
        return x
      }
    }
  };
</script>

<style scoped>
  /* 计数器 */
  .iCountUp {
    width: 100%;
    height: 100%;
    background: rgb(240, 243, 244);
    box-shadow: none;
    font-size: 20px;
    margin: 0;
    color: #4d63bc;
    font-weight: 300;
  }

  /* element ui */
  .el-row {
    margin-bottom: 20px;
  }
  .el-col {
    border-radius: 4px;
    width: 25%;
  }

  /* i 标签 */
  .el-col i {
    font-size: 60px;
  }
  .el-icon-s-custom {
    color: #40c9c6;
  }
  .el-icon-chat-dot-square {
    color: #36a3f7;
  }
  .el-icon-shopping-cart-2 {
    color: red;
  }
  .el-icon-wallet {
    color: #34bfa3;
  }

  /* 头部信息 */
  .wrap {
    background: #fff;
    display: flex;
    text-align: center;
    justify-content: space-around;
    align-items: center;
    margin: 10px;
    box-shadow: 0 2px 12px 0 rgba(0, 0, 0, 0.2);
    font-size: 1.5rem;
    font-weight: bolder;
    height: 131px;
  }
  .iCountUp .text {
    font-size: 0.8rem;
    color: #36a3f7;
    text-shadow: 0 5px 10px red;
    white-space: nowrap;
    margin-top: 20px;
    font-weight: 700;
  }
  .bg-purple {
    width: 50%;
    margin-top: -30px;
  }
  .bg-purple-light {
    background: #e5e9f2;
  }
  .grid-content {
    border-radius: 4px;
    min-height: 36px;
  }
  .row-bg {
    padding: 10px 0;
    background-color: #f9fafc;
  }

  /* charts */

  /* line */
  .linecharts {
    width: 98%;
    box-shadow: 0 2px 12px 0 rgba(0, 0, 0, 0.2);
    background: #ffffff;
    position: absolute;
    left: 200px;
  }
  /* pie */
  .pie {
    width: 98%;
    box-shadow: 0 2px 12px 0 rgba(0, 0, 0, 0.2);
    background: #ffffff;
    position: absolute;
    top: 600px;
    left: 200px;
  }

  /* bar */
  .bar {
    width: 98%;
    box-shadow: 0 2px 12px 0 rgba(0, 0, 0, 0.2);
    background: #ffffff;
    position: absolute;
    top: 1050px;
    left: 200px;
  }
</style>
