<template>
  <div>
    <el-scrollbar style="height:calc(100vh)">
      <el-menu
        :class="['el-menu-vertical',!isCollapse&&'noCollapse']"
        :collapse="isCollapse"
        :collapse-transition="true"
        :default-active="active"
        @select="selectMenuItem"
        active-text-color="#fff"
        background-color="#0F3D5F"
        text-color="rgb(191, 203, 217)"
        unique-opened
      >
        <template v-for="item in asyncRouters[0].children">
          <aside-component :key="item.name" :routerInfo="item" v-if="!item.hidden" />
        </template>
      </el-menu>
    </el-scrollbar>
  </div>
</template>

<script>
import { mapGetters } from 'vuex'
import AsideComponent from '@/view/layout/aside/asideComponent'
export default {
  name: 'Aside',
  data() {
    return {
      active: '',
      isCollapse: false
    }
  },
  methods: {
    selectMenuItem(index) {
      if (index === this.$route.name) return
      this.$router.push({ name: index })
    }
  },
  computed: {
    ...mapGetters('router', ['asyncRouters'])
  },
  components: {
    AsideComponent
  },
  created() {
    this.active = this.$route.name
    this.$bus.on('totalCollapse', () => {
      this.isCollapse = !this.isCollapse
    })
  },
  beforeDestroy() {
    this.$bus.off('totalCollapse')
  }
}
</script>

<style lang="scss">
.el-scrollbar {
  .el-scrollbar__view {
    height: 100%;
  }
}
.menu-info {
  .menu-contorl {
    line-height: 52px;
    font-size: 20px;
    display: table-cell;
    vertical-align: middle;
  }
}
</style>