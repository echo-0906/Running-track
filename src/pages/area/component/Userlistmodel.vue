<template>
    <div class="userdata" style="overflow:auto;" v-if="true">
      <div style="padding:10px; border-bottom:1px solid #000;">
        <span>框选内轨迹用户列表</span>
        <span class="close" style="float:right"><i class="el-icon-circle-close"></i></span>
      </div>
      <div class="tab2">
          <el-table :data="tableData" border style="width: 100%" >
            <el-table-column prop="usercont" width="180"></el-table-column>
            <el-table-column prop="uesrname" width="180"> </el-table-column>
          </el-table>
      </div>
    </div>
</template>
<script>
import { mapGetters } from "vuex";
export default {
  name: "userlist",
  data() {
    return {
        tableData: [{
          usercont:"",
          uesrname:""
        }],
        value1: '',
        value2: '',
        value3: '',
      };
  },
  methods: {
    handleClick(tab, event) {
        console.log(tab, event);
      }
  },
  computed: {},
  watch: {
    // dataRange(val) {
    // }
  }
};
</script>

<style lang="scss" scoped>
  .userdata{
  position: absolute;
  top: 100px;
  left: 550px;
  width: 400px;
  max-height: 500px;
  background-color: rgb(113, 128, 226);   
  }
</style>