<template>
    <div id="CesiumContainer" style="height: 100%;width: 100%;"></div>
    
  </template>
  
  <script>
  import * as Cesium from "cesium";
  import "cesium/Source/Widgets/widgets.css";
  
  Cesium.Ion.defaultAccessToken =
    `eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJqdGkiOiJmYjMxODI3Yi0yNDVlLTRjNzItYmExZC05ODI2YmU3YjhlZDIiLCJpZCI6MjA3MzU5LCJpYXQiOjE3MTI1ODIwNDl9.-vRShCftC9ZVCHMxS0ZRAlMi6Rw3mLp7YvhtF9ExH5o`;
  
  import { onMounted } from "vue";
  
  export default {
    name: 'HelloWorld',
    setup() {
      onMounted(() => {
        let viewer = new Cesium.Viewer('CesiumContainer', {
          baseLayerPicker: false,
          animation: false,
          infoBox: false,
        });
  
        viewer._cesiumWidget._creditContainer.style.display = "none";
  
        var start = new Cesium.JulianDate.fromDate(new Date());
        start = Cesium.JulianDate.addHours(start, 8, new Cesium.JulianDate());
        var stop = Cesium.JulianDate.addSeconds(start, 360, new Cesium.JulianDate());
  
        viewer.clock.startTime = start.clone();
        viewer.clock.stopTime = stop.clone();
        viewer.clock.currentTime = start.clone();
        viewer.clock.clockRange = Cesium.ClockRange.LOOP_STOP;
        viewer.clock.multiplier = 10;
        viewer.timeline.zoomTo(start, stop);
  
        function mySatePosition() {
          this.lon = 30; // 固定经度为30
          this.lat = 30; // 固定纬度为30
          this.satelliteHeight = 2100000;
          this.orbitHeight = 2100000 / 2;
          this.time = 0;
        }
  
        function computePosition(source, panduan) {
          var property = new Cesium.SampledPositionProperty();
          for (var i = 0; i < source.length; i++) {
            var time = Cesium.JulianDate.addSeconds(start, source[i].time, new Cesium.JulianDate());
            var position = Cesium.Cartesian3.fromDegrees(source[i].lon, source[i].lat, panduan === 1 ? source[i].satelliteHeight : source[i].orbitHeight);
            property.addSample(time, position);
          }
          return property;
        }
  
        function startSimulation() {
          let path = [new mySatePosition()]; // 使用固定位置的数据
          var entityPath = computePosition(path, 2);
          var entity = viewer.entities.add({
            availability: new Cesium.TimeIntervalCollection([new Cesium.TimeInterval({
              start: start,
              stop: stop
            })]),
            position: entityPath,
            orientation: new Cesium.VelocityOrientationProperty(entityPath),
            cylinder: {
              HeightReference: Cesium.HeightReference.CLAMP_TO_GROUND,
              length: 2100000,
              topRadius: 0,
              bottomRadius: 2100000 / 2,
              material: Cesium.Color.RED.withAlpha(0.4),
              outline: true,
              numberOfVerticalLines: 0,
              outlineColor: Cesium.Color.RED.withAlpha(0.8)
            },
          });
          entity.position.setInterpolationOptions({
            interpolationDegree: 5,
            interpolationAlgorithm: Cesium.LagrangePolynomialApproximation
          });
  
          var satellitePath = computePosition(path, 1);
          const modelUrl = "/satelite/scene.gltf";
          var satelliteEntity = viewer.entities.add({
            id: 'mySatellite',
            availability: new Cesium.TimeIntervalCollection([new Cesium.TimeInterval({
              start: start,
              stop: stop
            })]),
            position: satellitePath,
            orientation: new Cesium.VelocityOrientationProperty(satellitePath),
            model: {
              uri: modelUrl,
              minimumPixelSize: 100,
              maximumScale: 200000,
              scale: 1000.0,
            },
            path: {
              resolution: 1,
              material: new Cesium.PolylineGlowMaterialProperty({
                glowPower: 0.1,
                color: Cesium.Color.PINK
              }),
              width: 10
            }
          });
          satelliteEntity.position.setInterpolationOptions({
            interpolationDegree: 5,
            interpolationAlgorithm: Cesium.LagrangePolynomialApproximation
          });
        }
  
        startSimulation();
  
        var satelliteEntity = viewer.entities.getById('mySatellite');
      
  
        
  
        const groundStations = [
          { name: 'Ground Station 1', position: [116.4074, 39.9042] }, // 北京
          { name: 'Ground Station 2', position: [139.6917, 35.6895] }, // 东京
          { name: 'Ground Station 3', position: [55.3422, 25.2769] }, // 迪拜
          { name: 'Ground Station 4', position: [2.3522, 48.8566] }, // 巴黎
          { name: 'Ground Station 5', position: [31.2357, -25.7479] }, // 开普敦
          { name: 'Ground Station 6', position: [36.8219, -1.2921] }, // 内罗毕
          { name: 'Ground Station 7', position: [-46.6333, -23.5505] }, // 圣保罗
          { name: 'Ground Station 8', position: [-58.3816, -34.6037] }, // 布宜诺斯艾利斯
          { name: 'Ground Station 9', position: [-117.1611, 32.7157] }, // 圣地亚哥
          { name: 'Ground Station 10', position: [-79.3832, 43.6532] }, // 多伦多
          { name: 'Ground Station 11', position: [151.2093, -33.8688] }, // 悉尼
          { name: 'Ground Station 12', position: [174.7762, -36.8485] }, // 奥克兰
        ];
  
        const groundStationEntities = groundStations.map(station => {
          const position = Cesium.Cartesian3.fromDegrees(station.position[0], station.position[1]);
          return viewer.entities.add({
            name: station.name,
            position: position,
            box: {
              dimensions: new Cesium.Cartesian3(100000.0, 100000.0, 100000.0),
              material: Cesium.Color.RED,
            },
          });
        });
  
        let dashOffset = 0;
        const communicationRange = 5000000;
  
        const communicationLines = groundStationEntities.map(stationEntity => {
          return viewer.entities.add({
            polyline: {
              positions: new Cesium.CallbackProperty(() => {
                const satellitePosition = satelliteEntity.position.getValue(viewer.clock.currentTime);
                const stationPosition = stationEntity.position.getValue(viewer.clock.currentTime);
                if (satellitePosition && stationPosition) {
                  const distance = Cesium.Cartesian3.distance(satellitePosition, stationPosition);
                  if (distance <= communicationRange) {
                    return [satellitePosition, stationPosition];
                  }
                }
                return [];
              }, false),
              width: 2,
              material: new Cesium.PolylineGlowMaterialProperty({
                glowPower: 0.1,
                color: Cesium.Color.CYAN
              }),
              dashLength: 10,
              dashPattern: [10, 10],
              dashOffset: new Cesium.CallbackProperty(() => dashOffset, false),
            }
          });
        });
  
        viewer.scene.preRender.addEventListener(() => {
          dashOffset += 0.1;
        });
  
      });
    }
  };
  </script>
  