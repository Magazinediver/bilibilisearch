<template>
    <div class="sidebar">
        <el-menu
            class="sidebar-el-menu"
            :default-active="onRoutes"
            :collapse="collapse"
            background-color="#2f3a4b"
            text-color="#bfcbd9"
            active-text-color="#5ba3bb"
            unique-opened
            router
        >
            <template v-for="item in items">
                <template v-if="item.subs">
                    <el-submenu :index="item.index" :key="item.index">
                        <template slot="title">
                            <i :class="item.icon"></i>
                            <span slot="title">{{ item.title }}</span>
                        </template>
                        <template v-for="subItem in item.subs">
                            <el-submenu
                                v-if="subItem.subs"
                                :index="subItem.index"
                                :key="subItem.index"
                            >
                                <template slot="title">{{ subItem.title }}</template>
                                <el-menu-item
                                    v-for="(threeItem,i) in subItem.subs"
                                    :key="i"
                                    :index="threeItem.index"
                                >{{ threeItem.title }}</el-menu-item>
                            </el-submenu>
                            <el-menu-item
                                v-else
                                :index="subItem.index"
                                :key="subItem.index"
                            >{{ subItem.title }}</el-menu-item>
                        </template>
                    </el-submenu>
                </template>
                <template v-else>
                    <el-menu-item :index="item.index" :key="item.index">
                        <i :class="item.icon"></i>
                        <span slot="title">{{ item.title }}</span>
                    </el-menu-item>
                </template>
            </template>
        </el-menu>
    </div>
</template>

<script>
import bus from '../../assets/javascript/bus.js';
export default {
    data() {
        return {
          collapse: false,
            items: [
                {
                  icon: 'el-icon-house',
                  index: 'home',
                  title: '首页'
                },
                {
                  icon: 'el-icon-tickets',
                  index: 'allsearch',
                  title: '综合查询'
                },
                {
                  icon: 'el-icon-user-solid',
                  index: 'usersearch',
                  title: 'up主搜索'
                },
                {
                  icon: 'el-icon-film',
                  index: 'videosearch',
                  title: '视频搜索'
                },
                {
                  icon: 'el-icon-s-tools',
                  index: 'usermanager',
                  title: '用户管理',
                  subs: [
                    {
                      index: 'regist',
                      title: '注册新用户'
                    },
                    {
                      index: 'logout',
                      title: '注销'
                    },
                    {
                      index: 'changepwd',
                      title: '修改密码'
                    },
                    {
                      index: 'userfile',
                      title: '用户资料'
                    },
                  ]
                },
            ]
        };
    },
    computed: {
        onRoutes() {
            return this.$route.path.replace('/', '');
        }
    },
    created() {
      // 通过 Event Bus 进行组件间通信，来折叠侧边栏
      bus.$on('collapse', msg => {
        this.collapse = msg;
        bus.$emit('collapse-content', msg);
      });
    }
};
</script>

<style scoped>
.sidebar {
  display: block;
  position: absolute;
  left: 0;
  top: 60px;
  bottom: 0;


}
.sidebar::-webkit-scrollbar {
    width: 0;
}
.sidebar-el-menu:not(.el-menu--collapse) {
    width: 250px;
}
.sidebar > ul {
    height: 100%;
}


</style>
