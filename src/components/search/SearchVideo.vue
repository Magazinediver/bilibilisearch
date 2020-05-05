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
        <el-col :span="16">
          <el-input placeholder="请输入你想查询的up主" v-model="queryInfo.query" clearable @clear="getvideo">
            <el-button slot="append" icon="el-icon-search" @click="getvideo"></el-button>
          </el-input>
        </el-col>
      </el-row>

      <el-tabs v-model="activeName" @tab-click="handleClick" tab-position="top">
        <el-tab-pane label="最新发布" name="first">
          <Tabcomponent :cvideolist="videolist"></Tabcomponent>
        </el-tab-pane>
        <el-tab-pane label="最多播放" name="second">
          <Tabcomponent :cvideolist="videolist"></Tabcomponent>
        </el-tab-pane>
        <el-tab-pane label="最多硬币" name="third">
          <Tabcomponent :cvideolist="videolist"></Tabcomponent>
        </el-tab-pane>
        <el-tab-pane label="最多收藏" name="fourth">
          <Tabcomponent :cvideolist="videolist"></Tabcomponent>
        </el-tab-pane>
      </el-tabs>


      <!-- 分页区域 -->
      <el-pagination
        @size-change="handleSizeChange"
        @current-change="handleCurrentChange"
        :current-page="queryInfo.pagenum"
        :page-sizes="[12, 18, 20, 24]"
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
        //当前展开tab选项为
        activeName: 'first',
        currentDate: new Date(),

        //请求携带信息
        queryInfo: {
          query: '',
          // 当前页数
          pagenum: 1,
          // 每页显示多少数据
          pagesize: 20,

          tab : 'first'
        },

        // 视频列表
        videolist: [],
        // 总视频数
        totle: 0,

      }

    },
    created() {
      this.getvideo()
    },
    methods: {
      //回到首页函数
      aclick() {
        this.$router.push('/home');
      },

      //tab选项检测函数
      handleClick(tab, event) {
        this.tab = tab.name;
        // console.log(tab.name);
        // console.log(this.tab);
        console.log(tab, event);
        if(tab.name == 'second'){
          // 请求‘最多播放’视频
          this.getvideo();
        }else if(tab.name == 'third'){
          // 请求‘最多硬币’视频
          this.getvideo();
        }else if(tab.name == 'fourth'){
          // 请求‘最多收藏’视频
          this.getvideo();
        }else{
          // 请求‘最新发布’视频
          this.getvideo();
        }
      },


      async getvideo(){
        const { data: res } = await this.$http.get('/api/video/allvideomsg', {
          params: this.queryInfo
        })
        if (res.meta.status !== 200) {
          return this.$message.error('获取视频列表失败！')
        }
        this.videolist = res.data
        this.total = res.data.total
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



</style>
