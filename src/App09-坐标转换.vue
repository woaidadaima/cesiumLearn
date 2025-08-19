<template>
  <div id="cesiumContainer" class="container"></div>
</template>
<script setup>
//导入ION
import * as Cesium from "cesium";
window.CESIUM_BASE_URL = "/";

import "cesium/Build/Cesium/Widgets/widgets.css";
import { onMounted } from "vue";

Cesium.Ion.defaultAccessToken =
  "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJqdGkiOiJmYmE4NmE2OS1kNzYzLTRhZmItOThlMC05NjQxY2FiM2Y0OTQiLCJpZCI6MzIxMTU3LCJpYXQiOjE3NTI0NTc5Nzh9.R6dYKw8CWLxIFurs6-vr80vy28W5gztwaiT0fS5hn1M";

// 设置cesium默认视角
Cesium.Camera.DEFAULT_VIEW_RECTANGLE = Cesium.Rectangle.fromDegrees(
  // 西边的经度
  89.5,
  // 南边维度
  20.4,
  // 东边经度
  110.4,
  // 北边维度
  61.2
);

onMounted(() => {
  const viewer = new Cesium.Viewer("cesiumContainer", {
    infoBox: false,
  });

  viewer.cesiumWidget.creditContainer.style.display = "none";

  //监听点击事件
  window.addEventListener("click", (e) => {
    console.log("🚀 ~ e:", e);
    let Cartesian2 = new Cesium.Cartesian2(e.clientX, e.clientY);
    console.log("🚀 ~ Cartesian2:", Cartesian2);
  });

  //角度转弧度
  const radian = Cesium.Math.toRadians(90);
  console.log("🚀 ~ radian:", radian);
  //弧度转角度
  const angle = Cesium.Math.toDegrees(radian);
  console.log("🚀 ~ angle:", angle);

  //获取广州地理位置
  const position = Cesium.Cartesian3.fromDegrees(113.2644, 23.1291, 1000);
  console.log("🚀 ~ position:", position);

  const categraphi = Cesium.Cartographic.fromCartesian(position);
  console.log("🚀 ~ categraphi:", categraphi);

  console.log("layers", viewer.imageryLayers._layers, viewer.imageryLayers);
});
</script>
<style scoped lang="scss">
.container {
  width: 100vw;
  height: 100vh;
}
</style>
