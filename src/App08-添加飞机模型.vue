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

  //生产广州塔位置
  const position = Cesium.Cartesian3.fromDegrees(113.3191, 23.109, 2000);
  viewer.camera.flyTo({
    destination: position,
    orientation: {
      heading: Cesium.Math.toRadians(0), // 方向
    },
  });
  console.log(viewer.entities);

  const osmBuildings = await Cesium.createOsmBuildingsAsync();
  console.log("🚀 ~ osmBuildings:", osmBuildings);

  //添加3D建筑
  viewer.scene.primitives.add(osmBuildings);
  //添加一个实体点
  viewer.entities.add({
    position: Cesium.Cartesian3.fromDegrees(113.3191, 23.109, 800),
    point: {
      pixelSize: 10,
      color: Cesium.Color.RED,
      outlineColor: Cesium.Color.WHITE,
      outlineWidth: 2,
    },
    label: {
      text: "广州塔",
      font: "24px sans-serif",
      fillColor: Cesium.Color.WHITE,
      outlineColor: Cesium.Color.BLACK,
      outlineWidth: 3,
      pixelOffset: new Cesium.Cartesian2(0, -30),
    },

    //添加广告牌
    billboard: {
      image: "/texture/gzt.png",
      width: 64,
      height: 64,
      verticalOrigin: Cesium.VerticalOrigin.BOTTOM,
      pixelOffset: new Cesium.Cartesian2(0, -50),
    },
  });

  //添加飞机
  const airplaneEntity = viewer.entities.add({
    name: "Airplane",
    position: Cesium.Cartesian3.fromDegrees(113.3191, 23.109, 800),
    model: {
      uri: "/model/Air.glb", // 模型路径
      minimumPixelSize: 64, // 最小像素大小
      maximumScale: 200, // 最大缩放比例
      distanceDisplayCondition: new Cesium.DistanceDisplayCondition(0, 5000), // 显示距离条件
    },
  });
  console.log("🚀 ~ airplaneEntity:", airplaneEntity);
});
</script>
<style scoped lang="scss">
.container {
  width: 100vw;
  height: 100vh;
}
</style>
