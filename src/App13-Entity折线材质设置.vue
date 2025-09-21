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

onMounted(async () => {
  const viewer = new Cesium.Viewer("cesiumContainer", {
    infoBox: false,
  });

  viewer.cesiumWidget.creditContainer.style.display = "none";

  // let material = new Cesium.PolylineDashMaterialProperty({
  //   color: Cesium.Color.RED,
  //   dashLength: 16,
  // });
  // let material = new Cesium.PolylineGlowMaterialProperty({
  //   color: Cesium.Color.CYAN,
  //   glowPower: 0.6,
  //   taperPower: 0.5,
  // });
  let material = new Cesium.PolylineArrowMaterialProperty(Cesium.Color.LIME);
  //添加折线
  viewer.entities.add({
    id: "lineEntity",
    name: "lineEntity",
    polyline: {
      positions: Cesium.Cartesian3.fromDegreesArray([90, 40, 130, 40]),
      width: 5,
      material: material,
    },
  });
});
</script>
<style scoped lang="scss">
.container {
  width: 100vw;
  height: 100vh;
}
</style>
