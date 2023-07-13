<template>
  <div class="tabs">
    <!-- 首页无法被关闭 -->
    <el-tag
      type="success"
      :key="tag.name"
      size="medium"
      v-for="(tag, index) in tags"
      :closable="tag.name !== 'home'"
      :disable-transitions="false"
      @close="handleClose(tag, index)"
      @click="changeMenu(tag)"
      :effect="$route.name === tag.name ? 'dark' : 'plain'"
    >
      {{ tag.label }}
    </el-tag>
  </div>
</template>

<script>
import { mapState, mapMutations } from 'vuex'
export default {
  computed: {
    ...mapState({
      tags: state => state.tab.tabsList
    })
  },
  methods: {
    ...mapMutations(['closeTag']),
    // 点击tag删除功能
    handleClose(tag, index) {
      const length = this.tags.length - 1
      this.closeTag(tag)
      // 如果关闭的标签不是当前路由的话，就不跳转
      if (tag.name !== this.$route.name) {
        return
      }
      // 关闭的标签是最后一项，往左跳转
      if (index === length) {
        this.$router.push({ name: this.tags[index - 1].name })
      } else {
        // 否则往右跳转
        this.$router.push({ name: this.tags[index].name })
      }
    },
    // 点击tag跳转
    changeMenu(item) {
      // 获取路由路径
      this.$router.push({ name: item.name })
      this.$store.commit('selectMenu', item)
    }
  }
}
</script>

<style lang="scss" scoped>
.tabs {
  padding: 20px;
  .el-tag {
    margin-right: 1%;
    cursor: pointer;
  }
}
</style>
