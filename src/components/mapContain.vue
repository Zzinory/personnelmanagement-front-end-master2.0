<template>
  <div class="mapWrapper">
    <div id="mapContainer"></div>
    <div class="mapInfo">
      <h4 id="status"></h4>
      <hr />
      <div id="result"></div>
      <hr />
      <div>
        由于众多浏览器已不再支持非安全域的定位请求，为保位成功率和精度，请升级您的站点到HTTPS。
      </div>
    </div>
  </div>
</template>

<script>
import AMapLoader from "@amap/amap-jsapi-loader";

export default {
  name: "mapContain",
  
  methods: {
    initMap() {
      AMapLoader.load({
        key: "b59c490f61a694b9d7576dd864f74d6e", // 申请好的Web端开发者Key，首次调用 load 时必填
        version: "2.0", // 指定要加载的 JSAPI 的版本，缺省时默认为 1.4.15
        plugins: ["AMap.Geolocation"], // 需要使用的的插件列表，如比例尺'AMap.Scale'等
      })
        .then((AMap) => {
          this.map = new AMap.Map("mapContainer", {
            //设置地图容器id
            viewMode: "3D", //是否为3D地图模式
            zoom: 5, //初始化地图级别
            // center: [105.602725, 37.076636], //初始化地图中心点位置
          });
          let _this = this;
          AMap.plugin("AMap.Geolocation", function () {
            var geolocation = new AMap.Geolocation({
              enableHighAccuracy: true, //是否使用高精度定位，默认:true
              timeout: 10000, //超过10秒后停止定位，默认：5s
              position: "RB", //定位按钮的停靠位置
              buttonOffset: new AMap.Pixel(10, 20), //定位按钮与设置的停靠位置的偏移量，默认：Pixel(10, 20)
              zoomToAccuracy: true, //定位成功后是否自动调整地图视野到定位点
            });
            _this.map.addControl(geolocation);
            geolocation.getCurrentPosition(function (status, result) {
              if (status == "complete") {
                onComplete(result);
              } else {
                onError(result);
              }
            });
          });
          function onComplete(data) {
            document.getElementById("status").innerHTML = "定位成功";
            var str = [];
            str.push("定位结果：" + data.position);
            str.push("定位类别：" + data.location_type);
            if (data.accuracy) {
              str.push("精度：" + data.accuracy + " 米");
            } //如为IP精确定位结果则没有精度信息
            str.push("是否经过偏移：" + (data.isConverted ? "是" : "否"));
            document.getElementById("result").innerHTML = str.join("<br>");
          }
          //解析定位错误信息
          function onError(data) {
            document.getElementById("status").innerHTML = "定位失败";
            document.getElementById("result").innerHTML =
              "失败原因排查信息:" + data.message;
          }
        })
        .catch((e) => {
          console.log(e);
        });
    },
  },
  mounted() {
    //DOM初始化完成进行地图初始化
    this.initMap();
  },
};
</script>

<style scoped>
.mapWrapper{
  position: relative;

}
#mapContainer {
  padding: 0px;
  margin: 0px;
  width: 500px;
  height: 500px;
}
.mapInfo {
  padding: 0.75rem 1.25rem;
  margin-bottom: 1rem;
  border-radius: 0.25rem;
  position: absolute;
  top: 0;
  background-color: white;
  width: 10rem;
  border-width: 0;
  right: 0;
  font-size: 12px;
  box-shadow: 0 2px 6px 0 rgba(114, 124, 245, 0.5);
}
</style>