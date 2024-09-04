<template>
    <div class="word-cloud-chart" ref="chart" style="width: 100%; height: 100%;"></div>
  </template>
  
  <script>
  import * as echarts from 'echarts';
  import 'echarts-wordcloud';
  
  export default {
    name: 'WordCloudChart',
    props: {
      words: {
        type: Array,
        default: () => [
          { name: '环境变化检测', value: 1000 },
          { name: '气候变化', value: 800 },
          { name: '空气质量监测', value: 600 },
          { name: '水体污染监测', value: 400 },
          { name: '目标跟踪', value: 200 },
          { name: '边界监控', value: 1000 },
          { name: '重建规划', value: 800 },
          { name: '资源分配优化', value: 600 },
          { name: '风暴跟踪', value: 400 },
          { name: '损害评估', value: 200 },
          { name: '气象灾害预警', value: 1000 },
          { name: '海洋污染检测', value: 800 },
          { name: '风暴跟踪', value: 400 },
          { name: '海洋温度监测', value: 200 },
          { name: '海洋流动与波浪监测', value: 1000 },
          { name: '地震后评估', value: 600 }
        ]
      },
      interval: {
        type: Number,
        default: 1000 // 动画间隔时间，默认为1秒
      }
    },
    data() {
      return {
        chart: null,
        currentIndex: 0
      };
    },
    mounted() {
      this.initChart();
      // 移除或注释掉这行来取消动画
      // this.startAnimation();
    },
    beforeUnmount() {
      if (this.chart) {
        this.chart.dispose();
        this.chart = null;
      }
    },
    methods: {
      initChart() {
        this.chart = echarts.init(this.$refs.chart);
        let option = {
          tooltip: {
            show: true
          },
          series: [
            {
              type: 'wordCloud',
              shape: 'circle',
              sizeRange: [12, 30], // 设置字体大小范围
              rotationRange: [-90, 90], // 字体旋转范围
              rotationStep: 20, // 字体旋转的步长
              gridSize: 25, // 网格大小，数值越大单词间距越大
              drawOutOfBound: false, // 防止单词出界
              textStyle: {
                color: function () {
                  return 'rgb(' + [
                    Math.round(Math.random() * 160),
                    Math.round(Math.random() * 160),
                    Math.round(Math.random() * 160)
                  ].join(',') + ')';
                },
                emphasis: {
                  shadowBlur: 10,
                  shadowColor: '#333'
                }
              },
              data: this.words
            }
          ]
        };
        this.chart.setOption(option);
      }
      // 移除或注释掉 startAnimation 方法
      // startAnimation() {
      //   const seriesLength = this.words.length;
      //   setInterval(() => {
      //     this.chart.dispatchAction({
      //       type: 'downplay',
      //       seriesIndex: 0,
      //       dataIndex: this.currentIndex
      //     });
  
      //     this.currentIndex = (this.currentIndex + 1) % seriesLength;
  
      //     this.chart.dispatchAction({
      //       type: 'highlight',
      //       seriesIndex: 0,
      //       dataIndex: this.currentIndex
      //     });
  
      //     this.chart.dispatchAction({
      //       type: 'showTip',
      //       seriesIndex: 0,
      //       dataIndex: this.currentIndex
      //     });
      //   }, this.interval);
      // }
    }
  };
  </script>
  
  <style scoped>
  .word-cloud-chart {
    width: 100%;
    height: 100%;
  }
  </style>
  