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

  //通过entity创建矩形实体
  viewer.entities.add({
    name: "rectangle",
    rectangle: {
      coordinates: Cesium.Rectangle.fromDegrees(100, 20, 110, 30),
      material: Cesium.Color.RED.withAlpha(0.5),
    },
  });

  const rectangleGeometry = new Cesium.RectangleGeometry({
    rectangle: Cesium.Rectangle.fromDegrees(80, 20, 90, 30),
    vertexFormat: Cesium.PerInstanceColorAppearance.VERTEX_FORMAT,
  });

  const rectangleInstance = new Cesium.GeometryInstance({
    geometry: rectangleGeometry,
    attributes: {
      color: Cesium.ColorGeometryInstanceAttribute.fromColor(
        Cesium.Color.BLUE.withAlpha(0.5)
      ),
    },
  });
  const rectangleGeometry2 = new Cesium.RectangleGeometry({
    rectangle: Cesium.Rectangle.fromDegrees(90, 20, 100, 30),
    vertexFormat: Cesium.PerInstanceColorAppearance.VERTEX_FORMAT,
  });
  const rectangleInstance2 = new Cesium.GeometryInstance({
    geometry: rectangleGeometry2,
    attributes: {
      color: Cesium.ColorGeometryInstanceAttribute.fromColor(
        Cesium.Color.GREEN.withAlpha(0.5)
      ),
    },
  });
  //通过primitives创建矩形
  viewer.scene.primitives.add(
    new Cesium.Primitive({
      geometryInstances: [rectangleInstance, rectangleInstance2],
      appearance: new Cesium.PerInstanceColorAppearance({}),
    })
  );
});
</script>
<style scoped lang="scss">
.container {
  width: 100vw;
  height: 100vh;
}
</style>
