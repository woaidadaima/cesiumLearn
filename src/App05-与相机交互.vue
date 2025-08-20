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

  //弧度转角度
  const radian = Cesium.Math.toRadians(6.283185307179586);
  console.log("🚀 ~ radian:", radian);

  //生产北京天安门位置
  const position = Cesium.Cartesian3.fromDegrees(116.391128, 39.903527, 500);
  viewer.camera.flyTo({
    destination: position,
    orientation: {
      heading: Cesium.Math.toRadians(0), // 方向
      pitch: Cesium.Math.toRadians(-20), // 俯仰角
      // roll: Cesium.Math.toRadians(-180), // 翻滚角
    },
  });
  //监听按键按下

  window.addEventListener("keydown", (e) => {
    if (e.key === "d") {
      viewer.camera.moveRight(100);
    } else if (e.key === "a") {
      viewer.camera.moveLeft(100);
    } else if (e.key === "w") {
      viewer.camera.moveForward(100);
    } else if (e.key === "s") {
      viewer.camera.moveBackward(100);
    } else if (e.key === "q") {
      viewer.camera.lookLeft(Cesium.Math.toRadians(0.1));
    } else if (e.key === "e") {
      viewer.camera.lookRight(Cesium.Math.toRadians(0.1));
    } else if (e.key === "r") {
      viewer.camera.lookUp(Cesium.Math.toRadians(0.1));
    } else if (e.key === "f") {
      viewer.camera.lookDown(Cesium.Math.toRadians(0.1));
    } else if (e.key === "g") {
      viewer.camera.twistLeft(Cesium.Math.toRadians(0.1));
    } else if (e.key === "h") {
      viewer.camera.twistRight(Cesium.Math.toRadians(0.1));
    }
  });
  console.log("🚀 ~  viewer.camera:", viewer.camera);
});
</script>
<style scoped lang="scss">
.container {
  width: 100vw;
  height: 100vh;
}
</style>
