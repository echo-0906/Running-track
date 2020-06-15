<template>
    <div class="time" v-if="false">
      <div style="padding:10px ; border-bottom:1px solid #000">
        <span>查询选项</span>
        <span class="close" style="float:right"><i class="el-icon-circle-close"></i></span>
      </div>
      <div class="datebtn" style="margin-top:10px">
        <span style="margin:20px">日期:</span>
        <!-- <el-button type="text">昨天</el-button>
        <el-button type="text">今天</el-button>
        <el-button type="text">明天</el-button> -->
      </div>
      <div class="block" style="margin-top:10px">
          <el-date-picker v-model="value1" align="right" type="date"  placeholder="选择日期" :picker-options="pickerOptions" >
          </el-date-picker>
      </div>
      <div class="addtime" style="margin-top:10px">
          <div class="text" style="margin:10px ">
            <span style="margin-right:140px">开始时间</span>
            <span >结束时间</span>
          </div>
          <el-time-select v-model="value2" :picker-options="{ start: '08:30',  step: '00:15', end: '18:30'}" placeholder="选择时间">
          </el-time-select>
          <el-time-select v-model="value3" :picker-options="{ start: '08:30',  step: '00:15', end: '18:30'}" placeholder="选择时间">
          </el-time-select>
      </div>
      <div class="btn" style="margin-top:10px">
        <el-row>
          <el-button type="success" class="serbtn" style="margin:0px 80px" round>查询</el-button>
          <el-button type="success" class="delbtn" round>取消</el-button>
        </el-row>
      </div>     
    </div>
</template>
<script>
import { mapGetters } from "vuex";
export default {
  name: "query",
  data() {
    return {
        startTime: '',
        endTime: '',
        pickerOptions: {
          
          disabledDate(time) {
            return time.getTime() > Date.now();
          },
          shortcuts: [{
            text: '今天',
            onClick(picker) {
              picker.$emit('pick', new Date());
            }
          }, {
            text: '昨天',
            onClick(picker) {
              const date = new Date();
              date.setTime(date.getTime() - 3600 * 1000 * 24);
              picker.$emit('pick', date);
            }
          }, {
            text: '明天',
            onClick(picker) {
              const date = new Date();
              date.setTime(date.getTime() + 3600 * 1000 * 24);
              picker.$emit('pick', date);
            }
          }]
        },
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
  .time {
    position: absolute;
    top: 100px;
    right: 100px;
    width: 450px;
    height: 200px;
    background-color: rgb(248, 255, 240);
  }
</style>