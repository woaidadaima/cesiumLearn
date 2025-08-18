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
    geocoder: false,
    homeButton: false,
    sceneModePicker: false,
    timeline: false,
    navigationHelpButton: false,
    animation: false,
    fullscreenButton: false,
    baseLayerPicker: false,
    // baseLayer: new Cesium.ImageryLayer(
    //   new Cesium.WebMapTileServiceImageryProvider({
    //     url: `http://t0.tianditu.gov.cn/img_w/wmts?service=wmts&request=GetTile&version=1.0.0&layer=img&style=default&tileMatrixSet=w&format=tiles&TileMatrix={TileMatrix}&TileRow={TileRow}&TileCol={TileCol}&tk=417cf4b0fb088ad4db8fc8a78d77d41b`,
    //     layer: "img",
    //     style: "default",
    //     tileMatrixSetID: "w",
    //   })
    // ),
    //高德矢量地图
    baseLayer: new Cesium.ImageryLayer(
      new Cesium.UrlTemplateImageryProvider({
        url: "http://webrd02.is.autonavi.com/appmaptile?lang=zh_cn&size=1&scale=1&style=8&x={x}&y={y}&z={z}",
      })
    ),
    skyBox: new Cesium.SkyBox({
      sources: {
        positiveX: "/texture/sky/px.jpg",
        negativeX: "/texture/sky/nx.jpg",
        positiveY: "texture/sky/ny.jpg",
        negativeY: "texture/sky/py.jpg",
        positiveZ: "texture/sky/pz.jpg",
        negativeZ: "texture/sky/nz.jpg",
      },
    }),
    // selectedImageryProviderViewModel: new Cesium.ProviderViewModel({
    //   name: "天地图矢量图",
    //   iconUrl: "/texture/tianditu.png",
    //   tooltip: "天地图矢量图",
    //   creationFunction: () => {
    //     return new Cesium.WebMapTileServiceImageryProvider({
    //       url: `http://t0.tianditu.gov.cn/vec_w/wmts?service=wmts&request=GetTile&version=1.0.0&layer=vec&style=default&tileMatrixSet=w&format=tiles&TileMatrix={TileMatrix}&TileRow={TileRow}&TileCol={TileCol}&tk=417cf4b0fb088ad4db8fc8a78d77d41b`,
    //       layer: "vec",
    //       style: "default",
    //       format: "image/jpeg",
    //       tileMatrixSetID: "w",
    //     });
    //   },
    // }),
    // imageryProviderViewModels:
    //   Cesium.createDefaultImageryProviderViewModels().concat([
    //     new Cesium.ProviderViewModel({
    //       name: "天地图矢量图",
    //       iconUrl: "/texture/tianditu.png",
    //       tooltip: "天地图矢量图",
    //       creationFunction: () => {
    //         return new Cesium.WebMapTileServiceImageryProvider({
    //           url: `http://t0.tianditu.gov.cn/vec_w/wmts?service=wmts&request=GetTile&version=1.0.0&layer=vec&style=default&tileMatrixSet=w&format=tiles&TileMatrix={TileMatrix}&TileRow={TileRow}&TileCol={TileCol}&tk=417cf4b0fb088ad4db8fc8a78d77d41b`,
    //           layer: "vec",
    //           style: "default",
    //           format: "image/jpeg",
    //           tileMatrixSetID: "w",
    //         });
    //       },
    //     }),
    //   ]),
  });

  // const provider = new Cesium.WebMapTileServiceImageryProvider({
  //   url: `http://t0.tianditu.gov.cn/vec_w/wmts?service=wmts&request=GetTile&version=1.0.0&layer=vec&style=default&tileMatrixSet=w&format=tiles&TileMatrix={TileMatrix}&TileRow={TileRow}&TileCol={TileCol}&tk=417cf4b0fb088ad4db8fc8a78d77d41b`,
  //   layer: "vec",
  //   style: "default",
  //   format: "image/jpeg",
  //   tileMatrixSetID: "w",
  // });

  // const layer = new Cesium.ImageryLayer(provider);
  // viewer.imageryLayers.add(layer);手动添加图层

  viewer.cesiumWidget.creditContainer.style.display = "none";

  console.log("layers", viewer.imageryLayers._layers, viewer.imageryLayers);
});
</script>
<style scoped lang="scss">
.container {
  width: 100vw;
  height: 100vh;
}
</style>
