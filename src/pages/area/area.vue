<template>
  <div>
    <div id="area"></div>
    <query></query>
    <userinfo></userinfo>
    <userlist></userlist>
  </div>
</template>

<script>
import BMap from "BMap";
import { mapGetters } from "vuex";
import axios from "axios";
export default {
  name: "Area",
  data() {
    return {
      mapStyle: {
        style: "midnight"
      },
      map: null,
      drawingManager: null,
      isOpen: true,
      overlays: [],
      mlnglat: [],
      ply: "",
      scopePointsBd: []
    };
  },
  // components: {
  //   // 页面组件
  //   Search: () => import("./area/search"),
  // },

  // computed: {
  //   ...mapGetters(["status"]),
  //   dataRange() {
  //     const { status } = this;
  //     return { status };
  //   }
  // },
  // watch: {
  //   status(val) {
  //     if (val == true) {
  //       this.isOpend = true;
  //       this.initdrawingManager();
  //     }
  //   }
  // },
  methods: {
    //创建地图
    createMap() {
      this.map = new BMap.Map("area");

      this.map.centerAndZoom(new BMap.Point(114.31, 30.52), 10);
      this.map.enableScrollWheelZoom(true);
      // 为给定 ID 的 user 创建请求
      this.initdrawingManager();
      this.requestData();
      map.setCurrentCity("武汉");
    },
    //轨迹显示
    requestData() {
      let self = this;
      axios
        .get("/static/wuhan-car")
        .then(function(response) {
          var data = [];
          var timeData = [];
          response = response.data.split("\n");

          var maxLength = 0;
          for (var i = 0; i < response.length; i++) {
            var item = response[i].split(",");
            var coordinates = [];
            if (item.length > maxLength) {
              maxLength = item.length;
            }

            for (var j = 0; j < item.length; j += 2) {
              coordinates.push([item[j], item[j + 1]]);
              timeData.push({
                geometry: {
                  type: "Point",
                  coordinates: [item[j], item[j + 1]]
                },
                count: 1,
                time: j
              });
            }

            data.push({
              geometry: {
                type: "LineString",
                coordinates: coordinates,
                id: j,
                username: "user" + j
              }
            });
          }
          var dataSet = new mapv.DataSet(data);

          var options = {
            strokeStyle: "rgba(53,57,255,0.5)",
            coordType: "bd09mc",
            // globalCompositeOperation: 'lighter',
            shadowColor: "rgba(53,57,255,0.2)",
            shadowBlur: 3,
            lineWidth: 3.0,
            draw: "simple",
            methods: {
              click: function(item) {
                console.log(item);
              }
            }
          };

          var mapvLayer = new mapv.baiduMapLayer(self.map, dataSet, options);

          var dataSet = new mapv.DataSet(timeData);

          var options = {
            fillStyle: "rgba(194, 50, 50, 1)",
            coordType: "bd09mc",
            globalCompositeOperation: "lighter",
            size: 1.5,
            animation: {
              stepsRange: {
                start: 0,
                end: 100
              },
              trails: 3,
              duration: 5
            },
            draw: "simple"
          };

          var mapvLayer = new mapv.baiduMapLayer(self.map, dataSet, options);
        })
        .catch(function(error) {
          console.log(error);
        });
    },
    //------------------地图划区域-------------------------
    //百度地图绘图工具
    initdrawingManager() {
      var styleOptions = {
        strokeColor: "#0c93e0", // 边线颜色。
        fillColor: "#0c93e0", // 填充颜色。当参数为空时，圆形将没有填充效果。
        strokeWeight: 1, // 边线的宽度，以像素为单位。
        strokeOpacity: 0.8, // 边线透明度，取值范围0 - 1。
        fillOpacity: 0.1, // 填充的透明度，取值范围0 - 1。
        strokeStyle: "solid" // 边线的样式，solid或dashed。
      };
      // 实例化鼠标绘制工具
      this.drawingManager = new BMapLib.DrawingManager(this.map, {
        isOpen: false, // 是否开启绘制模式
        enableDrawingTool: true, // 是否显示工具栏
        drawingToolOptions: {
          anchor: BMAP_ANCHOR_TOP_LEFT, // 位置
          offset: new BMap.Size(5, 5), // 偏离值
          drawingModes: [
            BMAP_DRAWING_CIRCLE,
            BMAP_DRAWING_POLYGON,
            BMAP_DRAWING_RECTANGLE
          ]
        },
        circleOptions: styleOptions, // 圆的样式
        polygonOptions: styleOptions, // 多边形的样式
        rectangleOptions: styleOptions
      });
      // 添加鼠标绘制工具监听事件，用于获取绘制结果
      this.drawingManager.addEventListener(
        "overlaycomplete",
        this.overlaycomplete
      );
    },
    // 画线工具获取坐标点
    overlaycomplete(e) {
      //多边形选区
      if (
        e.drawingMode == BMAP_DRAWING_POLYLINE ||
        e.drawingMode == BMAP_DRAWING_POLYGON ||
        e.drawingMode == BMAP_DRAWING_RECTANGLE
      ) {
        var path = e.overlay.getPath();
        var polygonArr = new Array();
        for (var i in path) {
          polygonArr.push({
            longitude: path[i].lng,
            latitude: path[i].lat
          });
        }
        console.log(polygonArr);
        this.drawingManager.close();
      }
      //圆形选区
      if (e.drawingMode == BMAP_DRAWING_CIRCLE) {
        var Radius = e.overlay.getRadius();
        var CenterPoint =
          e.overlay.getCenter().lng + "," + e.overlay.getCenter().lat;
        console.log("半径=" + Radius);
        console.log("中心点=" + CenterPoint);
        this.drawingManager.close();
      }
    },
    //描述点
    scopePointsBdall(scopePointsBd) {
      array = JSON.parse(scopePointsBd);
      for (i = 0; i < array.length; i++) {
        var lng = array[i].lng;
        lat = array[i].lat;
        scopePointsBd.push(lng, lat);
      }
      // $("#eare_points").val(scopePointsBd);
    },
    //清除一类覆盖物
    removeOverlays(overlayType) {
      var allOverlays = map.getOverlays();
      for (var i = 0; i < allOverlays.length; i++) {
        if (allOverlays[i].toString() != overlayType) {
          map.removeOverlay(allOverlays[i]);
        }
      }
    },
    //重置覆盖物样式
    resetOverlay(overlayType) {
      for (var i = 0; i < _overlays.length; i++) {
        if (_overlays[i].toString() != overlayType) {
          _overlays[i].setStrokeWeight(1);
        }
      }
    }
  },
  components: {
    // 页面组件
    query: () => import("../area/component/Querymodel"),
    userinfo: () => import("../area/component/Userinfomodel"),
    userlist: () => import("../area/component/Userlistmodel")
  },
  mounted() {
    this.createMap();

    // this.showDrawingPanel();

    // //优先加载地图
    // setTimeout(() => {
    //   this.getHeatmap();
    // }, 200);
    // setTimeout(() => {
    //   this.getPath();
    // }, 200);
  }
};
</script>

<style lang="scss" scoped>
// #map {
// 	flex: auto;
// 	width: 100%;
// }
#area {
  height: 100vh;
  display: flex;
  flex-direction: column;
}
</style>
