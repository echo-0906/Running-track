<template>
  <div class="box">
    <el-tabs v-model="activeName" @tab-click="handleClick">
      <el-tab-pane label="轨迹回放&热力图" name="first">
        <Nav></Nav>
        <Map></Map>
      </el-tab-pane>
      <el-tab-pane label="区域搜索轨迹" name="second">
        <!-- <div class="info"> -->
        <Area></Area>
        <!-- </div> -->
      </el-tab-pane>
    </el-tabs>
  </div>
</template>

<script>
import { mapGetters } from "vuex";
export default {
  name: "index",
  data() {
    return {
      activeName: "first"
    };
	},
	 methods: {
      handleClick(tab, event) {
        console.log(tab, event);
      }
    },
  components: {
    // 页面组件
    Nav: () => import("./headnav/query"),
    Map: () => import("./map/map"),
    Area: () => import("./area/area"),
  },
  computed: {
    ...mapGetters(["input"]),
    dataRange() {
      const { input } = this;
      return { input };
    }
  },
  watch: {
    // dataRange(val) {
    // }
  }
};
</script>

<style lang="scss" scoped="scoped">
.box {
  height: 100vh;
  display: flex;
  flex-direction: column;
  position: relative;
}

</style>
