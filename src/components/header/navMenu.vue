<template>
  <el-menu default-active="0"
           class="el-menu"
           mode="horizontal">
    <el-menu-item v-for="(item, index) of hasMenuList"
                  :index="index + ''"
                  :key="item.text"
                  @click="item.http ? goHttp(item.http) : goRouter(item.path)">{{item.text}}
    </el-menu-item>
    <el-submenu :index="index + getIndex + ''"
                v-for="(item, index) of noMenuList"
                :key="item.text"
                popper-class="el-submenu">
      <template slot="title">{{item.text}}</template>
      <el-menu-item v-for="(i, ind) of item.children"
                    :key="i.text"
                    :index="`${index}-${ind}`"
                    @click="i.http ? goHttp(i.http) : goRouter(i.path)">
        {{i.text}}
      </el-menu-item>
    </el-submenu>

  </el-menu>
</template>

<script>
export default {
  data () {
    return {
      menuList: [{
        text: '首页',
        path: '/'
      },
      {
        text: '归档',
        path: '/archives'
      },
      
      {
        text: '笔记',
        http: 'http://note.qzran.cn'
      },
      {
        text: '项目',
        path: '',
        children: [{
          text: 'dnf折扣计算',
          path: '/project/calc'
        }, {
          text: '聊天室',
          path: '/project/websocket'
        }, {
          text: '俄罗斯方块',
          path: '/project/tetris'
        }]
      }, {
        text: '管理',
        admin: true,
        path: '',
        children: [{
          text: '编辑',
          path: '/admin/edit'
        }, {
          text: '管理',
          path: '/admin/index'
        }]
      },
      {
        text: '关于',
        path: '/about'
      },
        // {
        //   text: '状态',
        //   path: '/state'
        // }, 
      ]
    }
  },
  computed: {
    getIndex () {
      let index = 0
      for (const item of this.menuList) {
        if (item.children) index++
      }
      return index
    },
    authFilter () {
      const admin = this.$store.getters['user/getAdmin']
      if (admin) return this.menuList
      return this.menuList.filter(x => {
        return !x.admin
      })
    },
    hasMenuList () {
      return this.authFilter.filter(x => {
        return !x.children
      })
    },
    noMenuList () {
      return this.authFilter.filter(x => {
        return x.children
      })
    }
  },
  methods: {
    goRouter (path) {
      this.$router.push({ path })
    },
    goHttp(path) {
      this.$nextTick(() => {
        console.log(window)
        window.location.href = path
      })
    }
  }
}
</script>

<style lang="stylus" scoped>
.el-menu
  width 100%
.el-menu li
  width 8%
  display flex
  justify-content center
.el-submenu
  display flex
  justify-content center
</style>
