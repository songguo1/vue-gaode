<template>
  <div id="container"></div>
</template>

<script>
import { EventBus } from "../eventbus/event-bus";
import AMapLoader from "@amap/amap-jsapi-loader";
window._AMapSecurityConfig = {
  securityJsCode: "145241ffbe852d24530ba2ef4da01478",
};
export default {
  data() {
    return {
      map: null,
      autoOptions: {
        input: "",
      },
      auto: null,
      placeSearch: null,
    };
  },
  created() {
    EventBus.$on("shareinputId", (data) => this.handleinputId(data));
  },
  methods: {
    handleinputId(data) {
      this.autoOptions.input = data;
      console.log(this.autoOptions.input);
    },

    initMap() {
      AMapLoader.load({
        key: "7edd4c5b99b780f8c433e29dd972fbc7", // 申请好的Web端开发者Key，首次调用 load 时必填
        version: "2.0", // 指定要加载的 JSAPI 的版本，缺省时默认为 1.4.15
        plugins: [
          "AMap.ToolBar",
          "AMap.Scale",
          "AMap.ControlBar",
          "AMap.HawkEye",
          "AMap.MapType",
          "AMap.AutoComplete",
          "AMap.PlaceSearch",
        ],
      })
        .then((AMap) => {
          this.map = new AMap.Map("container", {
            //设置地图容器id
            resizeEnable: true,
            viewMode: "3D", //是否为3D地图模式
            zoom: 10, //初始化地图级别
            center: [121.473667, 31.230525], //初始化地图中心点位置
          });

          this.auto = new AMap.AutoComplete(this.autoOptions);
          // 添加比例尺控件
          this.map.addControl(new AMap.Scale());

          // 添加工具栏控件
          this.map.addControl(new AMap.ToolBar());

          // 添加地图类型切换控件
          this.map.addControl(new AMap.MapType());

          this.map.addControl(new AMap.HawkEye());

          this.map.addControl(new AMap.ControlBar());

          this.placeSearch = new AMap.PlaceSearch({
            map: this.map,
          });
          this.auto.on("select", (e)=>{
            //针对选中的poi实现自己的功能
            this.placeSearch.search(e.poi.name);
          });
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
#container {
  padding: 0px;
  margin: 0px;
  width: 100%;
  height: 100%;
  position: absolute;
}
</style>
