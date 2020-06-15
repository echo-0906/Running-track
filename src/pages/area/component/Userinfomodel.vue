<template>

    <div class="userinfo" style="overflow:auto ;" v-if="true">
      <div style="padding:10px; border-bottom:1px solid #000;">
        <span>用户详情</span>
        <span class="close" style="float:right"><i class="el-icon-circle-close"></i></span>
      </div>
      <div class="tab">
          <el-table :data="tableData" border style="width: 100%" >
            <el-table-column prop="date" width="180"></el-table-column>
            <el-table-column prop="name" width="180"> </el-table-column>
          </el-table>
      </div>
    </div>

</template>
<script>
import { mapGetters } from "vuex";
export default {
  name: "userinfo",
  data() {
    return {
        tableData: [{
          date: '用户一',
          name: '',
        },]

      };
  },
  methods: {
    handleClick(tab, event) {
        console.log(tab, event);
      },
      
  },
  computed: {},
  watch: {
    // dataRange(val) {
    // }
  }
};
</script>

<style lang="scss" scoped>
  .userinfo{
  position: absolute;
  top: 100px;
  left: 100px;
  width: 400px;
  max-height: 500px;
  background-color: rgb(113, 128, 226);   
  }
</style>