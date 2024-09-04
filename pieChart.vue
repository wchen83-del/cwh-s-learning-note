<template>  
    <div class="pie-Chart" ref="chart" style="width: 100%;height: 100%;padding: 10px;"></div>  
</template>

<script>
import * as echarts from 'echarts';

export default {
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
            this.chart.dispose();
            this.chart = null;
        }
        if (this.timer) {
            clearInterval(this.timer);
            this.timer = null;
        }
    },
    methods: {
        initChart() {
            this.chart = echarts.init(this.$refs.chart);

            let option = {
                title: {
                    text: '下载人员的来源组成',  // 主标题文本
                    
                    left: 'center',              // 标题居中对齐
                    textStyle: {                 // 设置主标题的样式
                    color:'#5CB1C1'             // 将主标题文本颜色设置为蓝色
             },
                    
                },
                tooltip: {
                    trigger: 'item'
                },
                legend: {
                    orient: 'vertical',
                    left: 'left'
                },
                series: [
                    {
                        name: 'Access From',
                        type: 'pie',
                        radius: '60%',
                        data: [
                            { value: 1048, name: '高校教师' },
                            { value: 735, name: '学生' },
                            { value: 580, name: '企业人员' },
                            { value: 484, name: '研究机构人员' },
                            { value: 300, name: '其他' }
                        ],
                        emphasis: {
                            itemStyle: {
                                shadowBlur: 10,
                                shadowOffsetX: 0,
                                shadowColor: 'rgba(0, 0, 0, 0.5)'
                            }
                        }
                    }
                ]
            };

            this.chart.setOption(option);
        },
        startAnimation() {
            const seriesLength = this.chart.getOption().series[0].data.length;

            this.timer = setInterval(() => {
                // 取消前一个扇区的高亮显示
                this.chart.dispatchAction({
                    type: 'downplay',
                    seriesIndex: 0,
                    dataIndex: this.currentIndex
                });

                // 更新当前索引
                this.currentIndex = (this.currentIndex + 1) % seriesLength;

                // 高亮当前扇区
                this.chart.dispatchAction({
                    type: 'highlight',
                    seriesIndex: 0,
                    dataIndex: this.currentIndex
                });

                // 将当前扇区显示在中心位置
                this.chart.dispatchAction({
                    type: 'showTip',
                    seriesIndex: 0,
                    dataIndex: this.currentIndex
                });
            }, 500); // 每2秒轮换一个扇区
        }
    }
};
</script>

<style scoped>
.pie-Chart {
    width: 100%;
    height: 100%;
}
</style>
