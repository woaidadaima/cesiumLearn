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
    // geocoder: false,
    // homeButton: false,
    // sceneModePicker: false,
    // timeline: false,
    // navigationHelpButton: false,
    // animation: false,
    // fullscreenButton: false,
    // baseLayerPicker: false,
    // terrain: Cesium.Terrain.fromWorldTerrain({
    //   requestWaterMask: true,
    //   requestVertexNormals: true,
    // }),
    terrainProvider: await Cesium.CesiumTerrainProvider.fromUrl(
      "/terrains/gz",
      {
        requestWaterMask: true,
        requestVertexNormals: true,
      }
    ),
  });

  viewer.cesiumWidget.creditContainer.style.display = "none";
  // 相机飞到广州
  viewer.camera.flyTo({
    destination: Cesium.Rectangle.fromDegrees(
      113.2, // 西经
      22.5, // 南纬
      114.5, // 东经
      23.5 // 北纬
    ),
  });

  console.log("layers", viewer.imageryLayers._layers, viewer.imageryLayers);
});
</script>
<style scoped lang="scss">
.container {
  width: 100vw;
  height: 100vh;
}
</style>
