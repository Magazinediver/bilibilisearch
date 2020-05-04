<template>
  <div>
    <div class="sec_bar">
        <header class="page-header">
          <h2>首页</h2>

          <div style="margin-right: 40px" class="right-wrapper pull-right">
            <ol class="breadcrumbs">
              <li>
              <span @click="aclick()">
                <i class="fa fa-home"></i>
              </span>
              </li>
              <li><span>首页</span></li>
            </ol>

            <a class="sidebar-right-toggle" ></a>
          </div>
        </header>
    </div>
    <!-- 2.为Echarts准备一个Dom -->
    <div id="content-body-index">
      <div id="bilibili_2233">
        <img src="~assets/images/bilibili_2233.png" >
      </div>
      <div id="bilibili_search">
        <div class="search-control-wrapper">
          <form action="pages-search-results.html">
            <div class="form-group">
              <div id="index-search" class="input-group">
                <input type="text" class="form-control bilibili-input" placeholder="你想搜索些什么...">
                <span class="input-group-btn bilibili-span">
                  <button type="submit" id="bilibili-button" class="btn btn-primary">搜索</button>
                </span>
              </div>
            </div>
          </form>
        </div>
      </div>

      <div class="index-suggest">
        <div class="home-suggest">
          <div class="hot-search">
            <div class="title"><i class="title-icon hot"></i>热门搜索<span class="subtitle">Daily Hot</span></div>
            <ul class="hotlist">
              <li class="item" v-for="(item,index) in special_daily" :key="index+'1'">
                <span class="num special">{{index+1}}</span>
                <span class="word">{{item}}</span>
              </li>
              <li class="item" v-for="(item,index) in daily1" :key="index+'2'">
                <span class="num">{{index+4}}</span>
                <span class="word">{{item}}</span>
              </li>
            </ul>
            <ul class="hotlist">
              <li class="item" v-for="(item,index) in daily2" :key="index+'3'">
                <span class="num">{{index+11}}</span>
                <span class="word">{{item}}</span>
              </li>
            </ul>
          </div>
          <div class="history">
            <div class="title"><i class="title-icon time"></i>搜索历史<span class="delete-history"><i class="icon-garbage"></i>清空</span>
              <span class="subtitle">History</span>
            </div>
            <ul class="list" v-for="(item,index) in history" :key="index">
              <li class="item">
                <a href="javascript:;">{{item}}</a>
              </li>
            </ul>
            <div class="no-history" style="display: none;">
              <span>没有历史记录</span>
            </div>
          </div>
        </div>
      </div>

    </div>

  </div>

</template>

<script>
// 1.导入echarts
import echarts from 'echarts'

export default {
  // 此时,页面上的元素,已经被渲染完毕了
  async mounted () {

  },
  data(){
    return{
      special_daily: ['仙王的日常生活','周深','淡黄的长裙'],
      daily1: ['青春有你2','星游记','特朗普','老番茄','世卫组织','四月新番',
        '达拉崩吧'],
      daily2:[
        '籽岷','中国boy','刺客五六七','张大仙','约会大作战',
        '地缚少年花子君','蜡笔小新','钉钉','成果','回形针'
      ],
      history:['黄驿涵','黄科烨','赵焕明']
    }
  },
  methods:{
    aclick(){
      this.$router.push('/home');
    }
  }
}
</script>

<style>
  @import "../assets/stylesheets/theme.css";
  .sec_bar{
    display: block;
    width:100%;
    margin-top: 40px;
    margin-left: 40px;
  }

  #content-body-index{
    padding-right: 70px;
  }

</style>
