<template>
  <div>
    <!-- 导航区 -->
    <section style="width: 2000px;padding-bottom: 0;padding-top: 40px;" role="main" class="content-body">
      <header class="page-header">
        <h2>综合查询</h2>

        <div class="right-wrapper pull-right">
          <ol class="breadcrumbs">
            <li>
              <span @click="aclick()">
                <i class="fa fa-home"></i>
              </span>
            </li>
            <li><span>综合查询</span></li>
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
          <el-input placeholder="开始您的搜索吧" v-model="queryInfo.query" clearable @clear="getvideo"  @keyup.enter.native="getvideo">
            <el-button class="allsearch" slot="append" icon="el-icon-search" @click="getvideo"></el-button>
          </el-input>
        </el-col>
      </el-row>



      <el-row :gutter="20" style="margin-top: 15px">
        <el-col :span="18" :offset="3">
          <ul class="box">
            <li v-for="(item,index) of videotype" :class="{checked:index==queryInfo.select}" @click="changeList(index)">{{item}}</li>
          </ul>
        </el-col>
      </el-row>

      <el-row :gutter="20" style="margin-top: 15px">
        <el-col :span="18" :offset="3">
          <ul class="box2">
            <li v-for="(item,index) of emotiontype" :class="{checked:index==queryInfo.selectemotion}" @click="changeListEmotion(index)">{{item}}</li>
          </ul>
        </el-col>
      </el-row>

      <!-- 搜索结果列表区域 -->
      <el-row :gutter="20" style="margin-top: 15px">
        <el-col :span="18" :offset="3">
          <el-tabs v-model="activeName" @tab-click="handleClick" tab-position="top" stretch>
            <el-tab-pane label="搜索/推荐" name="first">
              <user-item :cdis="parentdisplay" :cup="up"></user-item>
              <Tabcomponent :cquery="this.query" :cvideolist="videolist"></Tabcomponent>
            </el-tab-pane>
            <el-tab-pane label="最新发布" name="second">
              <Tabcomponent :cquery="this.query" :cvideolist="videolist"></Tabcomponent>
            </el-tab-pane>
            <el-tab-pane label="最多播放" name="third">
              <Tabcomponent :cquery="this.query" :cvideolist="videolist"></Tabcomponent>
            </el-tab-pane>
            <el-tab-pane label="最多弹幕" name="fourth">
              <Tabcomponent :cquery="this.query" :cvideolist="videolist"></Tabcomponent>
            </el-tab-pane>
            <el-tab-pane label="最多硬币" name="fifth">
              <Tabcomponent :cquery="this.query" :cvideolist="videolist"></Tabcomponent>
            </el-tab-pane>
            <el-tab-pane label="最多收藏" name="sixth">
              <Tabcomponent :cquery="this.query" :cvideolist="videolist"></Tabcomponent>
            </el-tab-pane>
          </el-tabs>
        </el-col>
      </el-row>







      <!-- 分页区域 -->
      <el-row :gutter="20" style="margin-top: 15px">
        <el-col :span="18" :offset="3">
          <el-pagination
            @size-change="handleSizeChange"
            @current-change="handleCurrentChange"
            :current-page="queryInfo.pagenum"
            :page-sizes="[2, 5, 10, 15]"
            :page-size="queryInfo.pagesize"
            layout="total, sizes, prev, pager, next, jumper"
            :total="total"
          ></el-pagination>
        </el-col>
      </el-row>
    </el-card>


  </div>
</template>

<script>
  import Tabcomponent from "./videocomponent/VideoItem";
  import UserItem from "../user/UserItem";

  export default {
    components:{
      Tabcomponent,
      UserItem,
    },
    name: "SearchAll",
    data () {
      return {
        parentdisplay: 'nodis',
        query:'',
        videotype : ["全部分区","动漫","舞蹈","番剧","时尚","游戏","鬼畜","国创","生活","音乐","科技","数码","影视","娱乐"],
        emotiontype : ["全部情感","令人愉悦","令人悲伤","励志","恐怖","失落"],

        //当前展开tab选项为
        activeName: 'first',
        currentDate: new Date(),

        //请求携带信息
        queryInfo: {
          select: 0,
          selectemotion: 0,
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
        up : [],
        // 总视频数


      }

    },
    created() {
      this.queryInfo.query = this.$store.state.query
      console.log(this.queryInfo.query);
      this.getvideo()

    },
    methods: {

      changeList(index){
        this.queryInfo.select = index;//this指向app
        this.getvideo();
      },

      changeListEmotion(index){
        this.queryInfo.selectemotion = index;//this指向app
        this.getvideo();
      },


      //回到首页函数
      aclick() {
        this.$router.push('/home');
      },

      //tab选项检测函数
      handleClick(tab, event) {
        this.queryInfo.tab = tab.name;
        // console.log(tab.name);
        // console.log(this.tab);
        // console.log(tab, event);
        this.getvideo();
      },


      async getvideo(){
        const { data: res } = await this.$http.get('/bilibili/video/upandvideo', {
          params: this.queryInfo
        })
        if (res.meta.status !== 200) {
          return this.$message.error('获取搜索结果失败！')
        }
        this.up = res.data.up
        this.videolist = res.data.videos
        this.total = res.total
        this.query = this.queryInfo.query

        if(this.up !== null){
          this.parentdisplay = 'dis'
        }else{
          this.parentdisplay = 'nodis'
        }
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

  .box{
    margin-left: -10px;
    padding:0;
    list-style:none;
  }

  .box  li{
    width: fit-content;
    height:23px;
    display:inline-block;
    border-radius:4px;
    text-align:center;
    line-height:23px;
    cursor:pointer;
    transition:all 0.3s linear;
    margin-left:10px;
    padding-left: 5px;
    padding-right: 5px;
  }
  .box li:hover{
    color: #00a1d6;
  }
  .box li.checked{
    background-color: #00a1d6;
    color: #fff;
  }

  .box2{
    margin-left: -10px;
    padding:0;
    list-style:none;
  }

  .box2  li{
    width: fit-content;
    height:23px;
    display:inline-block;
    border-radius:4px;
    text-align:center;
    line-height:23px;
    cursor:pointer;
    transition:all 0.3s linear;
    margin-left:10px;
    padding-left: 5px;
    padding-right: 5px;
  }
  .box2 li:hover{
    color: #00a1d6;
  }
  .box2 li.checked{
    background-color: #00a1d6;
    color: #fff;
  }

  .el-input-group__append{
    background-color: #fb7299!important;
    color: white !important;
  }

  .allsearch{
    background-color: #fb7299 !important;
    color: white !important;
  }

  .allsearch:hover{
    background-color: #fd457a!important;
    color: white !important;
  }

</style>
