<template>
  <div class="line-Chart" ref="chart" style="width: 100%; height: 100%;"></div>
</template>

<script>
import * as echarts from 'echarts';

export default {
  name: 'MyCylingderChart',
  data() {
    return {
      chart: null,
      currentIndex: -1,
      timer: null
    };
  },
  mounted() {
    this.initChart();
    this.startAnimation();
  },
  onBeforeUnmount() {
    if (this.chart) {
      this.chart.dispose(); // 在组件销毁前，销毁 echarts 实例以释放资源  
      this.chart = null; // 清空引用  
    }
    if (this.timer) {
      clearInterval(this.timer);
      this.timer = null;
    }
  },
  updated() {
    // 如果你的数据和配置可能会在组件更新时发生变化，你可以在这里重新渲染图表  
    this.initChart();
  },
  methods: {
    initChart() {
      const chartDom = this.$refs.chart;
      this.chart = echarts.init(chartDom);

      // 下载量柱状图数据
      const barSeries = [
        {
          name: '研究机构',
          type: 'bar',
          stack: '下载量',
          data: [30, 50, 40, 20, 10],
          itemStyle: {
            color: '#1f77b4', // 蓝色
           
            shadowBlur: 10, // 阴影模糊
            shadowOffsetX: 5, // 阴影水平偏移
            shadowOffsetY: 5, // 阴影垂直偏移
            shadowColor: 'rgba(0, 0, 0, 0.2)' // 阴影颜色
          }
        },
        {
          name: '高校教师',
          type: 'bar',
          stack: '下载量',
          data: [20, 40, 30, 10, 20],
          itemStyle: {
            color: '#ff7f0e', // 橙色
            
            shadowBlur: 10, // 阴影模糊
            shadowOffsetX: 5, // 阴影水平偏移
            shadowOffsetY: 5, // 阴影垂直偏移
            shadowColor: 'rgba(0, 0, 0, 0.2)' // 阴影颜色
          }
        },
        {
          name: '学生',
          type: 'bar',
          stack: '下载量',
          data: [50, 70, 60, 40, 30],
          itemStyle: {
            color: '#2ca02c', // 绿色
            shadowBlur: 10, // 阴影模糊
            shadowOffsetX: 5, // 阴影水平偏移
            shadowOffsetY: 5, // 阴影垂直偏移
            shadowColor: 'rgba(0, 0, 0, 0.2)' // 阴影颜色
          }
        },
        {
          name: '企业人员',
          type: 'bar',
          stack: '下载量',
          data: [10, 20, 15, 30, 40],
          itemStyle: {
            color: '#d62728', // 红色
            shadowBlur: 10, // 阴影模糊
            shadowOffsetX: 5, // 阴影水平偏移
            shadowOffsetY: 5, // 阴影垂直偏移
            shadowColor: 'rgba(0, 0, 0, 0.2)' // 阴影颜色
          }
        },
        {
          name: '其他',
          type: 'bar',
          stack: '下载量',
          data: [10, 20, 5, 10, 10],
          itemStyle: {
            color: '#9467bd', // 紫色
            borderRadius: [5, 5, 0, 0], // 圆角
            shadowBlur: 10, // 阴影模糊
            shadowOffsetX: 5, // 阴影水平偏移
            shadowOffsetY: 5, // 阴影垂直偏移
            shadowColor: 'rgba(0, 0, 0, 0.2)' // 阴影颜色
          }
        }
      ];

      // 折线图数据 - 生产量
      const lineSeries = {
        data: [100, 180, 130, 90, 60], // 生产量数据
        type: 'line', // 折线图类型
        name: '生产量', // 类别名称
        itemStyle: {
          color: '#FF5733' // 生产量折线的颜色
        },
        lineStyle: {
          width: 2, // 线条宽度
          color: '#FF5733' // 线条颜色
        },
        smooth: true // 使线条圆滑
      };

      // 图表选项配置
      let option = {
        title: {
          text: '近五个月卫星的数据量', // 标题文本
          left: 'center', // 标题居中
          top: '5%', // 距离顶部位置
          textStyle: {
            color: '#20abe2', // 标题颜色
            fontSize: 18, // 标题字体大小
          }
        },
        tooltip: {
          trigger: 'axis' // 坐标轴触发
        },
        legend: {
          data: ['研究机构', '高校教师', '学生', '企业人员', '其他', '生产量'], // 图例数据
          bottom: '10px' // 图例位置
        },
        xAxis: {
          type: 'category',
          data: ['May', 'June', 'July', 'August', 'September'] // 横坐标近五个月的月份
        },
        yAxis: {
          type: 'value'
        },
        series: [...barSeries, lineSeries] // 包含堆叠柱状图和折线图的 series
      };

      this.chart.setOption(option);
    },
    startAnimation() {
      const seriesLength = this.chart.getOption().series[0].data.length;

      this.timer = setInterval(() => {
        // 取消前一个数据项的高亮显示
        this.chart.dispatchAction({
          type: 'downplay',
          seriesIndex: 0,
          dataIndex: this.currentIndex
        });

        // 更新当前索引
        this.currentIndex = (this.currentIndex + 1) % seriesLength;

        // 高亮当前数据项
        this.chart.dispatchAction({
          type: 'highlight',
          seriesIndex: 0,
          dataIndex: this.currentIndex
        });

        // 将当前数据项显示在中心位置
        this.chart.dispatchAction({
          type: 'showTip',
          seriesIndex: 0,
          dataIndex: this.currentIndex
        });
      }, 1000); // 每1秒轮换一个数据项
    }
  }
}
</script>

<style scoped>
.line-Chart {
  width: 100%;
  height: 100%;
}
</style>
