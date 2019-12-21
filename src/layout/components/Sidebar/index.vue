<template>
  <div :class="{'has-logo':showLogo}">
    <logo v-if="showLogo" :collapse="isCollapse" />
    <el-scrollbar wrap-class="scrollbar-wrapper">
      <el-menu
        :default-active="activeMenu"
        :collapse="isCollapse"
        :background-color="variables.menuBg"
        :text-color="variables.menuText"
        :unique-opened="false"
        :active-text-color="variables.menuActiveText"
        :collapse-transition="false"
        mode="vertical"
      >
        <sidebar-item v-for="route in slideList" :key="route.path" :item="route" :base-path="route.path" />
      </el-menu>
    </el-scrollbar>
  </div>
</template>

<script>
import { mapGetters } from 'vuex'
import Logo from './Logo'
import SidebarItem from './SidebarItem'
import variables from '@/styles/variables.scss'

export default {
  components: {
    SidebarItem,
    Logo
  },
  computed: {
    ...mapGetters([
      'permission_routes',
      'sidebar'
    ]),

    activeMenu() {
      const route = this.$route
      const { meta, path } = route
      if (meta.activeMenu) {
        return meta.activeMenu
      }
      return path
    },

    showLogo() {
      return this.$store.state.settings.sidebarLogo
    },

    variables() {
      return variables
    },

    isCollapse() {
      return !this.sidebar.opened
    }
  },

  // permission_routes

  data() {
    return {
      slideList: [
        /* {
                    children: [],
                    meta: {
                        icon: 'component',
                        title: "首页",
                    },
                    name: 'TinymceDemo',
                    path: 'tinymce',
                    redirect: 'noRedirect'
                },*/
        {
          children: [],
          meta: {
            icon: 'component',
            title: '客户管理'
          },
          name: '客户管理',
          path: 'console/customerManage',
          redirect: 'noRedirect'
        },
        {
          children: [],
          meta: {
            icon: 'component',
            title: '充提记录'
          },
          name: '充提记录',
          path: 'console/ctRecords',
          redirect: 'noRedirect'
        },
        {
          children: [],
          meta: {
            icon: 'component',
            title: '运营报表'
          },
          name: 'report',
          path: 'console/report',
          redirect: 'noRedirect'
        },
        {
          children: [],
          meta: {
            icon: 'component',
            title: '运营账户管理'
          },
          name: 'accountManage',
          path: 'console/accountManage',
          redirect: 'noRedirect'
        }
        /* {
                    children: [],
                    meta: {
                        icon: 'component',
                        title: '工作台',
                    },
                    name: 'MarkdownDemo',
                    path: 'markdown',
                    redirect: 'noRedirect'
                },
                {
                    children: [],
                    meta: {
                        icon: 'component',
                        title: '成员管理',
                    },
                    name: 'MarkdownDemo',
                    path: 'markdown',
                    redirect: 'noRedirect'
                },
                {
                    children: [],
                    meta: {
                        icon: 'component',
                        title: '积分记录',
                    },
                    name: 'MarkdownDemo',
                    path: 'markdown',
                    redirect: 'noRedirect'
                },
                {
                    children: [],
                    meta: {
                        icon: 'component',
                        title: '牌局记录',
                    },
                    name: 'MarkdownDemo',
                    path: 'markdown',
                    redirect: 'noRedirect'
                },
                {
                    children: [],
                    meta: {
                        icon: 'component',
                        title: '提现管理',
                    },
                    name: 'MarkdownDemo',
                    path: 'markdown',
                    redirect: 'noRedirect'
                },
                {
                    children: [
                        {
                            meta: {
                                title: '群组配置',
                            },
                            name: 'MarkdownDemo',
                            path: 'markdown',
                        },
                        {
                            meta: {
                                title: '规则配置',
                            },
                            name: 'MarkdownDemo',
                            path: 'markdown',
                        },
                        {
                            meta: {
                                title: '支付配置',
                            },
                            name: 'MarkdownDemo',
                            path: 'markdown',
                        },
                    ],
                    meta: {
                        icon: 'component',
                        title: '配置管理',
                    },
                    name: 'MarkdownDemo',
                    path: 'markdown',
                    redirect: 'noRedirect'
                },*/
      ]
    }
  },
  mounted() {
    console.log(this.permission_routes)
  }
}
</script>
