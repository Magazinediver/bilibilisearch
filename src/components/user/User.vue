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
        <el-col :span="6">
          <el-input placeholder="请输入你想查询的up主" v-model="queryInfo.query" clearable @clear="getuplist">
            <el-button slot="append" icon="el-icon-search" @click="getuplist"></el-button>
          </el-input>
        </el-col>
      </el-row>
      <!-- 用户列表区域 -->
      <el-table :data="uplist" border stripe>
        <!-- stripe: 斑马条纹
        border：边框-->
        <el-table-column type="index" label="#"></el-table-column>
        <el-table-column prop="mid" label="用户id"></el-table-column>
        <el-table-column prop="name" label="用户名"></el-table-column>
<!--        <el-table-column label="头像" align="center">-->
<!--          <template slot-scope="scope">-->
<!--            <el-image-->
<!--              style="width: 40px; height: 40px"-->
<!--              :src="scope.row.face"-->
<!--              :fit="fit">-->
<!--            </el-image>-->
<!--          </template>-->
<!--        </el-table-column>-->
        <el-table-column label="头像(查看大图)" align="center">
          <template slot-scope="scope">
            <el-image
              style="border-radius: 50%;width: 60px;height: 60px"
              class="table-td-thumb"
              :src="scope.row.face"
              :preview-src-list="[scope.row.face]"
            ></el-image>
          </template>
        </el-table-column>
        <el-table-column prop="sign" label="签名"></el-table-column>
        <el-table-column prop="level" label="等级"></el-table-column>
        <el-table-column prop="vipStatus" label="官方认证"></el-table-column>
        <el-table-column prop="following" label="关注"></el-table-column>
        <el-table-column prop="fans" label="粉丝"></el-table-column>
        <el-table-column prop="videoPlay" label="播放量"></el-table-column>
        <el-table-column prop="likes" label="点赞"></el-table-column>
      </el-table>
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
export default {
  data () {
    return {
      // 获取用户列表查询参数对象
      queryInfo: {
        query: '',
        // 当前页数
        pagenum: 1,
        // 每页显示多少数据
        pagesize: 5
      },
      uplist: [],
      total: 1,


      // 所有角色数据列表
      rolesLsit: [],
      // 已选中的角色Id值
      selectRoleId: ''
    }
  },
  created () {
    this.getuplist()
  },
  methods: {
    aclick() {
      this.$router.push('/home');
    },
    async getuplist () {
      const { data: res } = await this.$http.get('/api/up/allupmsg', {
        params: this.queryInfo
      })
      if (res.meta.status !== 200) {
        return this.$message.error('获取用户列表失败！')
      }
      this.uplist = res.data
      this.total = res.data.total
    },
    // 监听 pagesize改变的事件
    handleSizeChange (newSize) {
      // console.log(newSize)
      this.queryInfo.pagesize = newSize
      this.getuplist()
    },
    // 监听 页码值 改变事件
    handleCurrentChange (newSize) {
      // console.log(newSize)
      this.queryInfo.pagenum = newSize
      this.getuplist()
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

<style lang="less" scoped>
</style>
