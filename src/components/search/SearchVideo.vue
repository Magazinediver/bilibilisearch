<template>
  <div>

    <!-- 导航区 -->
    <section style="width: 2000px;padding-bottom: 0;padding-top: 40px;" role="main" class="content-body">
      <header class="page-header">
        <h2>视频查询</h2>

        <div class="right-wrapper pull-right">
          <ol class="breadcrumbs">
            <li>
              <span @click="aclick()">
                <i class="fa fa-home"></i>
              </span>
            </li>
            <li><span>视频查询</span></li>
          </ol>

          <a class="sidebar-right-toggle" ></a>
        </div>
      </header>
    </section>


    <!-- 卡片视图 -->
    <el-card style="margin-top: -40px">
      <!-- 搜索 添加 -->
      <el-row :gutter="20">
        <el-col :span="18" :offset="3">
          <el-input placeholder="请输入你想查询的视频" v-model="queryInfo.query">
            <el-select style="width: 130px" v-model="queryInfo.select" slot="prepend" placeholder="请选择">
              <el-option label="番剧" value="1"></el-option>
              <el-option label="视频" value="2"></el-option>
              <el-option label="影视" value="3"></el-option>

            </el-select>
            <el-button class="videosearch" slot="append" icon="el-icon-search" @click="getvideo"></el-button>
          </el-input>
        </el-col>
      </el-row>


      <el-row :gutter="20" style="margin-top: 15px">
        <el-col :span="18" :offset="3">
          <el-tabs v-model="activeName" @tab-click="handleClick" tab-position="top" stretch>
            <el-tab-pane label="综合搜索" name="first">
              <Tabcomponent :cvideolist="videolist"></Tabcomponent>
            </el-tab-pane>
            <el-tab-pane label="最新发布" name="second">
              <Tabcomponent :cvideolist="videolist"></Tabcomponent>
            </el-tab-pane>
            <el-tab-pane label="最多播放" name="third">
              <Tabcomponent :cvideolist="videolist"></Tabcomponent>
            </el-tab-pane>
            <el-tab-pane label="最多弹幕" name="fourth">
              <Tabcomponent :cvideolist="videolist"></Tabcomponent>
            </el-tab-pane>
            <el-tab-pane label="最多硬币" name="fifth">
              <Tabcomponent :cvideolist="videolist"></Tabcomponent>
            </el-tab-pane>
            <el-tab-pane label="最多收藏" name="sixth">
              <Tabcomponent :cvideolist="videolist"></Tabcomponent>
            </el-tab-pane>
          </el-tabs>
        </el-col>
      </el-row>

      <!-- 分页区域 -->
      <el-pagination
        @size-change="handleSizeChange"
        @current-change="handleCurrentChange"
        :current-page="queryInfo.pagenum"
        :page-sizes="[12, 18, 20, 24]"
        :page-size="queryInfo.pagesize"
        background
        layout="total, sizes, prev, pager, next, jumper"
        :total="total"
      ></el-pagination>
    </el-card>


  </div>
</template>

<script>
  import Tabcomponent from "./videocomponent/VideoItem";

  export default {
    components:{
      Tabcomponent,
    },
    name: "SearchAll",
    created() {
      this.queryInfo.query = this.$store.state.query
      console.log(this.queryInfo.query);
      this.getvideo()
    },
    data () {
      return {
        //当前展开tab选项为
        activeName: 'first',
        currentDate: new Date(),

        //请求携带信息
        queryInfo: {
          select: '1',
          query: '',
          // 当前页数
          pagenum: 1,
          // 每页显示多少数据
          pagesize: 20,
          tab : 'first',
        },
        total: 0,

        // 视频列表
        videolist: [],
        // 总视频数


      }

    },
    methods: {


      //回到首页函数
      aclick() {
        this.$router.push('/home');
      },

      //tab选项检测函数
      handleClick(tab, event) {
        this.queryInfo.tab = tab.name;
        // console.log(tab, event);
        // console.log(tab.name);
        // console.log(this.queryInfo.tab);
        this.getvideo()

      },


      async getvideo(){
        const { data: res } = await this.$http.get('/api/video/videomsg', {
          params: this.queryInfo
        })
        if (res.meta.status !== 200) {
          return this.$message.error('获取视频列表失败！')
        }
        this.videolist = res.data
        this.total = res.total
      },




      // 监听 pagesize改变的事件
      handleSizeChange (newSize) {
        // console.log(newSize)
        this.queryInfo.pagesize = newSize
        this.getvideo();
      },
      // 监听 页码值 改变事件
      handleCurrentChange (newSize) {
        // console.log(newSize)
        this.queryInfo.pagenum = newSize
        this.getvideo();
      },
    }
  }
</script>

<style scoped>

  .el-input-group__append{
    background-color: #fb7299!important;
    color: white !important;
  }

  .videosearch{
    background-color: #fb7299 !important;
    color: white !important;
  }

  .videosearch:hover{
    background-color: #fd457a!important;
    color: white !important;
  }

</style>
