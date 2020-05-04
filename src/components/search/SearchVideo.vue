<template>
  <div>
    <!-- 导航区 -->
    <section style="width: 2000px;padding-bottom: 0;padding-top: 40px;" role="main" class="content-body">
      <header class="page-header">
        <h2>用户查询</h2>

        <div class="right-wrapper pull-right">
          <ol class="breadcrumbs">
            <li>
              <span @click="aclick()">
                <i class="fa fa-home"></i>
              </span>
            </li>
            <li><span>UP主查询</span></li>
          </ol>

          <a class="sidebar-right-toggle" ></a>
        </div>
      </header>
    </section>


    <!-- 卡片视图 -->
    <el-card style="margin-top: -40px">
      <!-- 搜索 添加 -->
      <el-row :gutter="20">
        <el-col :span="16">
          <el-input placeholder="请输入你想查询的up主" v-model="queryInfo.query" clearable @clear="getUserList">
            <el-button slot="append" icon="el-icon-search" @click="getUserList"></el-button>
          </el-input>
        </el-col>
      </el-row>

      <el-tabs v-model="activeName" @tab-click="handleClick" tab-position="top">
        <el-tab-pane label="最新发布" name="first">
          <Tabcomponent></Tabcomponent>
        </el-tab-pane>
        <el-tab-pane label="最多播放" name="second">配置管理</el-tab-pane>
        <el-tab-pane label="最多硬币" name="third">角色管理</el-tab-pane>
        <el-tab-pane label="最多收藏" name="fourth">定时任务补偿</el-tab-pane>
      </el-tabs>


      <!-- 分页区域 -->
      <el-pagination
        @size-change="handleSizeChange"
        @current-change="handleCurrentChange"
        :current-page="queryInfo.pagenum"
        :page-sizes="[2, 5, 10, 15]"
        :page-size="queryInfo.pagesize"
        layout="total, sizes, prev, pager, next, jumper"
        :total="totle"
      ></el-pagination>
    </el-card>


  </div>
</template>

<script>
  import Tabcomponent from "./tabComponent/Tabcomponent";

  export default {
    components:{
      Tabcomponent,
    },
    name: "SearchAll",
    data () {
      return {
        activeName: 'first',
        // 获取用户列表查询参数对象
        currentDate: new Date(),
        queryInfo: {
          query: '',
          // 当前页数
          pagenum: 1,
          // 每页显示多少数据
          pagesize: 5
        },
        userlist: [],
        totle: 0,




        // 所有角色数据列表
        rolesLsit: [],
        // 已选中的角色Id值
        selectRoleId: '',


      }

    },

    methods: {
      handleClick(tab, event) {
        console.log(tab, event);
        if(tab.name == 'second'){
          // 触发‘配置管理’事件
          this.second();
        }else{
          // 触发‘用户管理’事件
          this.first();
        }
      },
      /**
       * 触发‘用户管理’事件
       */
      first(){
        console.log('我是用户管理');
      },
      /**
       * 触发‘配置管理’事件
       */
      second(){
        console.log('我是配置管理');
      },

      aclick() {
        this.$router.push('/home');
      },
      async getUserList () {
        const { data: res } = await this.$http.get('users', {
          params: this.queryInfo
        })
        if (res.meta.status !== 200) {
          return this.$message.error('获取用户列表失败！')
        }
        this.userlist = res.data.users
        this.totle = res.data.totle
      },
      // 监听 pagesize改变的事件
      handleSizeChange (newSize) {
        // console.log(newSize)
        this.queryInfo.pagesize = newSize
        this.getUserList()
      },
      // 监听 页码值 改变事件
      handleCurrentChange (newSize) {
        // console.log(newSize)
        this.queryInfo.pagenum = newSize
        this.getUserList()
      },
      // 监听 switch开关 状态改变
      async userStateChanged (userInfo) {
        // console.log(userInfo)
        const { data: res } = await this.$http.put(
          `users/${userInfo.id}/state/${userInfo.mg_state}`
        )
        if (res.meta.status !== 200) {
          userInfo.mg_state = !userInfo.mg_state
          return this.$message.error('更新用户状态失败')
        }
        this.$message.success('更新用户状态成功！')
      },
      // 监听 添加用户对话框的关闭事件
      addDialogClosed () {
        this.$refs.addUserFormRef.resetFields()
      },


    }
  }
</script>

<style scoped>



</style>
