<template>
  <div>
    <!-- 导航区 -->
    <section style="width: 2000px;padding-bottom: 0;padding-top: 40px;" role="main" class="content-body">
      <header class="page-header">
        <h2>视频排行榜</h2>

        <div class="right-wrapper pull-right">
          <ol class="breadcrumbs">
            <li>
              <span @click="aclick()">
                <i class="fa fa-home"></i>
              </span>
            </li>
            <li>
              <i style="font-size: 21px;padding-right: 5px" class="el-icon-data-analysis"></i>
              <span>视频排行</span>
            </li>
          </ol>

          <a class="sidebar-right-toggle" ></a>
        </div>
      </header>
    </section>

    <el-card style="margin-top: -40px">
      <!-- 搜索 添加 -->
      <el-row :gutter="20">
        <el-col :span="20":offset="2">
          <el-tabs v-model="activeName" @tab-click="handleClick" stretch>
            <el-tab-pane label="动漫" name="first">
              <rank-item :cranklist="ranklist"></rank-item>
            </el-tab-pane>
            <el-tab-pane label="舞蹈" name="second">
              <rank-item :cranklist="ranklist"></rank-item>
            </el-tab-pane>
            <el-tab-pane label="番剧" name="third">
              <rank-item :cranklist="ranklist"></rank-item>
            </el-tab-pane>
            <el-tab-pane label="时尚" name="fourth">
              <rank-item :cranklist="ranklist"></rank-item>
            </el-tab-pane>
            <el-tab-pane label="游戏" name="fifth">
              <rank-item :cranklist="ranklist"></rank-item>
            </el-tab-pane>
            <el-tab-pane label="鬼畜" name="sixth">
              <rank-item :cranklist="ranklist"></rank-item>
            </el-tab-pane>
            <el-tab-pane label="国创" name="seventh">
              <rank-item :cranklist="ranklist"></rank-item>
            </el-tab-pane>
            <el-tab-pane label="生活" name="eighth">
              <rank-item :cranklist="ranklist"></rank-item>
            </el-tab-pane>
            <el-tab-pane label="音乐" name="nineth">
              <rank-item :cranklist="ranklist"></rank-item>
            </el-tab-pane>
            <el-tab-pane label="科技" name="tenth">
              <rank-item :cranklist="ranklist"></rank-item>
            </el-tab-pane>
            <el-tab-pane label="数码" name="eleventh">
              <rank-item :cranklist="ranklist"></rank-item>
            </el-tab-pane>
            <el-tab-pane label="影视" name="twelfth">
              <rank-item :cranklist="ranklist"></rank-item>
            </el-tab-pane>
            <el-tab-pane label="娱乐" name="thirteenth">
              <rank-item :cranklist="ranklist"></rank-item>
            </el-tab-pane>
          </el-tabs>
        </el-col>
      </el-row>




    </el-card>


  </div>
</template>

<script>
  import RankItem from "./videocomponent/RankItem";

  export default {
    name: "Rank",
    components:{
       RankItem,
    },
    created() {
      this.getrank()
    },
    data () {

      return {
        //当前展开tab选项为
        activeName: 'first',
        currentDate: new Date(),

        //请求携带信息
        queryInfo: {
          tab : 'first',
        },
        // 视频列表
        ranklist: [
          {
            link : 'https://www.bilibili.com/video/BV1Xi4y1x7eM',
            title : '[何同学]我用一万行备忘录做了个动画...',
            pic : 'https://i0.hdslb.com/bfs/archive/f995d4da2cb83d3952c1fb3ea130af2e4bf9da02.jpg@114w_70h.webp',
            play : '233.3万',
            danmu : '2.0万',
            up : '老师好我叫何同学',
            score : '10000',
            mid : '163637592'
          },
          {
            link : 'https://www.bilibili.com/video/BV1Xi4y1x7eM',
            title : '[何同学]我用一万行备忘录做了个动画...',
            pic : 'https://i0.hdslb.com/bfs/archive/f995d4da2cb83d3952c1fb3ea130af2e4bf9da02.jpg@114w_70h.webp',
            play : '233.3万',
            danmu : '2.0万',
            up : '老师好我叫何同学',
            score : '10000',
            mid : '163637592'
          },
          {
            link : 'https://www.bilibili.com/video/BV1Xi4y1x7eM',
            title : '[何同学]我用一万行备忘录做了个动画...',
            pic : 'https://i0.hdslb.com/bfs/archive/f995d4da2cb83d3952c1fb3ea130af2e4bf9da02.jpg@114w_70h.webp',
            play : '233.3万',
            danmu : '2.0万',
            up : '老师好我叫何同学',
            score : '10000',
            mid : '163637592'
          },
          {
            link : 'https://www.bilibili.com/video/BV1Xi4y1x7eM',
            title : '[何同学]我用一万行备忘录做了个动画...',
            pic : 'https://i0.hdslb.com/bfs/archive/f995d4da2cb83d3952c1fb3ea130af2e4bf9da02.jpg@114w_70h.webp',
            play : '233.3万',
            danmu : '2.0万',
            up : '老师好我叫何同学',
            score : '10000',
            mid : '163637592'
          },
          {
            link : 'https://www.bilibili.com/video/BV1Xi4y1x7eM',
            title : '[何同学]我用一万行备忘录做了个动画...',
            pic : 'https://i0.hdslb.com/bfs/archive/f995d4da2cb83d3952c1fb3ea130af2e4bf9da02.jpg@114w_70h.webp',
            play : '233.3万',
            danmu : '2.0万',
            up : '老师好我叫何同学',
            score : '10000',
            mid : '163637592'
          },






        ]
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
        this.getrank()
      },


      async getrank(){
        const { data: res } = await this.$http.get('/bilibili/video/videomsg', {
          params: this.queryInfo
        })
        if (res.meta.status !== 200) {
          return this.$message.error('获取视频列表失败！')
        }else if(res.status !== 200){
          return this.$message.error('获取视频列表失败！')
          console.log('获取视频列表失败！');
        }
        this.ranklist = res.data
      },

    }
  }
</script>

<style scoped>

</style>
