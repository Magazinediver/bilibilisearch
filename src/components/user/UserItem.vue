<template>
  <div style="margin-right: 280px;margin-left: 40px">
    <ul class="list-unstyled up-list">
      <li class="user-item">
        <div class="has-thumb search-item">
          <div class="up-face">
            <a :href="up.link" title="狂气的芙兰" target="_blank" class="face-img">
              <div class="up-head">
                <img  :src="cup.face" alt="" />
              </div>
            </a>
          </div>
          <div class="result-data">
            <div style="width: 100%;height: 40px">
              <a :href="up.link" :title="up.name" target="_blank" style="float: left;text-decoration:none" class="up-name">{{cup.name}}</a>
              <p style="float: left;margin-left: 20px;margin-top: 3px" class="result-type">
                <span class="label label-primary" style="position: relative;margin-top: 40px">lv{{up.lv}}</span>
              </p>
            </div>

            <div class="up-info clearfix" style="margin-bottom: 6px">
              <span>稿件：{{up.videonum}}</span>
              <span>粉丝：{{up.fans}}</span>
            </div>
            <p class="description">{{up.sign}}</p>
          </div>
        </div>
<!--        <div class="has-thumb search-item">-->
<!--          <div class="up-face">-->
<!--            <a :href="cup.link" title="狂气的芙兰" target="_blank" class="face-img">-->
<!--              <div class="up-head">-->
<!--                <img  :src="cup.head" alt="" />-->
<!--              </div>-->
<!--            </a>-->
<!--          </div>-->
<!--          <div class="result-data">-->
<!--            <div style="width: 100%;height: 40px">-->
<!--              <a :href="cup.link" :title="cup.name" target="_blank" style="float: left;text-decoration:none" class="up-name">{{cup.name}}</a>-->
<!--              <p style="float: left;margin-left: 20px;margin-top: 3px" class="result-type">-->
<!--                <span class="label label-primary" style="position: relative;margin-top: 40px">lv{{cup.lv}}</span>-->
<!--              </p>-->
<!--            </div>-->

<!--            <div class="up-info clearfix" style="margin-bottom: 6px">-->
<!--              <span>稿件：{{cup.videonum}}</span>-->
<!--              <span>粉丝：{{cup.fan}}</span>-->
<!--            </div>-->
<!--            <p class="description">{{cup.des}}</p>-->
<!--          </div>-->
<!--        </div>-->
      </li>
    </ul>
  </div>
</template>

<script>
  export default {
    name: "UserItem",
    props:['cup'],

    data(){
      return{
        up:
          {
            lv : 5,
            link : '//space.bilibili.com/119?from=search',
            head : '//i2.hdslb.com/bfs/face/bc5ca101313d4db223c395d64779e76eb3482d60.jpg@315w_315h.webp',
            name : '老番茄',
            videonum : 184,
            fans : 3902,
            sign : '一日小明行至星光学院见一金发大眼挡在路中，小明定睛观瞧，此人右手饭勺木纹匀细有海苔味，身披星光学院校服头顶红色蝴蝶结，内衬天使软糖内衣脚蹬白色练功靴，腰间消防斧发出淡淡光芒，小明见罢心中暗想:此人莫非'
          }
      }

    }
  }
</script>

<style scoped>
  .up-list{
    display: block;
    padding-inline-start: 20px;
  }

  .user-item {
    margin-top: -10px;
    position: relative;
    padding: 20px 0 20px 102px;
    border-bottom: 1px solid #e5e9ef;
    z-index: 1;
  }

  .face-img {
    display: block;
    width: 100%;
    height: 100%;
    border-radius: 50%;
    overflow: hidden;

    text-decoration: none;
    outline: 0;
    color: #00a1d6;
    cursor: pointer;
  }

  .up-face{
    position: absolute;
    width: 90px;
    height: 90px;
    left: 0;
    top: 20px;
  }

  .up-head{
    width: 90px;
    height: 90px;
    display: inline-block;
  }

  .up-head img{
    display: block;
    width: 100%;
    height: 100%;
    border-radius: 50%;
    vertical-align: middle;
  }



  .up-name{
    font-weight: bold;
    margin-top: 5px;
    font-size: 20px;
  }

</style>
