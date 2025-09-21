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

  // let material = new Cesium.CheckerboardMaterialProperty({
  //   evenColor: Cesium.Color.WHITE,
  //   oddColor: Cesium.Color.BLACK,
  //   repeat: new Cesium.Cartesian2(4, 4),
  // });
  let material = new Cesium.StripeMaterialProperty({
    evenColor: Cesium.Color.WHITE,
    oddColor: Cesium.Color.BLACK,
    repeat: 32,
  });
  //通过entity创建矩形实体
  viewer.entities.add({
    id: "rectangleEntity",
    name: "rectangle",
    rectangle: {
      coordinates: Cesium.Rectangle.fromDegrees(100, 20, 110, 30),
      // material: Cesium.Color.RED.withAlpha(0.5),
      material: material,
    },
  });

  const rectangleGeometry = new Cesium.RectangleGeometry({
    rectangle: Cesium.Rectangle.fromDegrees(80, 20, 90, 30),
    vertexFormat: Cesium.EllipsoidSurfaceAppearance.VERTEX_FORMAT,
  });

  const rectangleInstance = new Cesium.GeometryInstance({
    id: "rectangleInstance1",
    geometry: rectangleGeometry,
    attributes: {
      color: Cesium.ColorGeometryInstanceAttribute.fromColor(
        Cesium.Color.BLUE.withAlpha(0.5)
      ),
    },
  });
  const rectangleGeometry2 = new Cesium.RectangleGeometry({
    rectangle: Cesium.Rectangle.fromDegrees(90, 20, 100, 30),
    vertexFormat: Cesium.EllipsoidSurfaceAppearance.VERTEX_FORMAT,
  });
  const rectangleInstance2 = new Cesium.GeometryInstance({
    id: "rectangleInstance2",
    geometry: rectangleGeometry2,
    attributes: {
      color: Cesium.ColorGeometryInstanceAttribute.fromColor(
        Cesium.Color.GREEN.withAlpha(0.5)
      ),
    },
  });
  // let material2 = Cesium.Material.fromType("Color");
  // material2.uniforms.color = new Cesium.Color(0.0, 1.0, 0.0, 0.5);
  // let appearance = new Cesium.EllipsoidSurfaceAppearance({
  //   material: material2,
  // });
  // let material2 = Cesium.Material.fromType("Color", {
  //   color: new Cesium.Color(0.0, 1.0, 1.0, 0.5),
  // });
  // let material2 = new Cesium.Material({
  //   fabric: {
  //     type: "Image",
  //     uniforms: {
  //       image: "/texture/logo.png",
  //       repeat: new Cesium.Cartesian2(1, 1),
  //     },
  //   },
  // });
  // let material2 = Cesium.Material.fromType("Image", {
  //   image: "/texture/logo.png",
  //   repeat: new Cesium.Cartesian2(2, 2),
  // });
  //type：DiffuseMap
  // let material2 = Cesium.Material.fromType("DiffuseMap", {
  //   image: "/texture/logo.png",
  //   // repeat: new Cesium.Cartesian2(2, 2),
  //   color: new Cesium.Color(0.0, 1.0, 1.0, 0.5),
  // });
  //type:grid
  // let material2 = Cesium.Material.fromType("Grid", {
  //   color: new Cesium.Color(0.0, 1.0, 1.0, 0.5),
  //   cellAlpha: 0.5,
  //   lineCount: new Cesium.Cartesian2(4, 4),
  //   lineThickness: new Cesium.Cartesian2(4.0, 4.0),
  //   lineOffset: new Cesium.Cartesian2(0.5, 0.5),
  // });

  //type:water
  let material2 = Cesium.Material.fromType("Water", {
    baseWaterColor: Cesium.Color.AQUA.withAlpha(0.8),
    distortion: 0.25,
    normalMap: "/Assets/Textures/waterNormals.jpg",
  });
  //可以采用上面传对象写法，也可以采用下面的方式
  // material2.uniforms.color = new Cesium.Color(0.0, 1.0, 1.0, 0.5);

  let appearance = new Cesium.EllipsoidSurfaceAppearance({
    material: material2,
  });
  const primitive = new Cesium.Primitive({
    geometryInstances: [rectangleInstance, rectangleInstance2],
    appearance: appearance,
  });

  //通过primitives创建矩形
  viewer.scene.primitives.add(primitive);
});
</script>
<style scoped lang="scss">
.container {
  width: 100vw;
  height: 100vh;
}
</style>
