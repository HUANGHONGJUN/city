<script setup>
import * as Cesium from "cesium";
import { onMounted } from "vue";
onMounted(() => {
  // const viewer = new Cesium.Viewer('cesiumContainer');
  var custom = new Cesium.ArcGisMapServerImageryProvider({
    url: "//services.arcgisonline.com/ArcGIS/rest/services/World_Street_Map/MapServer",
  });

  var viewer = new Cesium.Viewer("cesiumContainer", {
    baseLayerPicker: false,
    // //图像提供
    imageryProvider: custom,
    // //地形提供
    terrainProvider: Cesium.createWorldTerrain({
      requestWaterMask: true,
      requestVertexNormals: true,
    }),
  });

  viewer.camera.setView({
    //设置初始化视图
    destination: new Cesium.Cartesian3(1332761, -4662399, 4137888),
    //方向，俯视喝仰视的视角
    orientation: {
      heading: 0.6,
      pitch: -0.66,
    },
  });
  var city = viewer.scene.primitives.add(
    new Cesium.Cesium3DTileset({
      url: Cesium.IonResource.fromAssetId(3839),
    })
  );

  //定义3️d样式

  var heightstyle = new Cesium.Cesium3DTileStyle({
    color: {
      conditions: [
        ["${height}>=300", "rgba(45,0,75,0.5)"],
        ["${height}>=200", "rgba(102,71,151,0.5)"],
        ["${height}>=100", "rgba(170,162,204,0.5)"],
        ["${height}>=50", "rgba(224,226,238,0.5)"],
        ["${height}>=25", "rgba(224,230,200,0.5)"],
        ["${height}>=10", "rgba(248,176,87,0.5)"],
        ["${height}>=5", "rgba(198,106,11,0.5)"],
        ['true','rgb(127,59,8)']
      ],
    },
  });

  city.style = heightstyle;

  
 
});
</script>

<template>
  <div id="cesiumContainer"></div>
</template>

<style>
* {
  margin: 0;
  padding: 0;
}
#cesiumContainer {
  width: 100%;
  height: 100%;
  margin: 0;
  padding: 0;
  overflow: hidden;
}
</style>
