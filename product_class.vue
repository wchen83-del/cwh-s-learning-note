<template>
    <div class="threeBarChart" ref="chart"> </div>
</template>

<script>
import * as echarts from 'echarts';
export default {
    name: 'TestChart',
    mounted() {
        let myChart = echarts.init(this.$refs.chart); 
        let option = {
            title: {
                text: '0～4级产品的月数据量',  // 设置图表的主标题
                left: 'center',      // 标题水平居中
                top: '5%',          // 标题垂直居上
                textStyle: {         // 设置标题的文本样式
                    color: '#5CB1C1',  // 标题颜色
                    fontSize: 16,      // 标题字体大小
                    fontWeight: 'bold' // 标题字体粗细
                }
            },
            grid: {
                top: "20%",
                bottom: "15%",
                left: 40,
                right: 40,
            },
            tooltip: {
                trigger: 'axis'
            },
            legend: {
                left: "8%",
                top: "10%",
                itemWidth: 7,
                itemHeight: 7,
                textStyle: {
                    color: '#5CB1C1',
                    fontSize: 10
                }
            },
            calculable: true,
            xAxis: [
                {
                    type: 'category',
                    axisLine: {
                        symbol: ['none', 'arrow'],
                        symbolSize: [6, 6],
                        lineStyle: {
                            color: '#122C49'
                        }
                    },
                    axisLabel: {
                        color: '#61B9C8',
                        showMaxLabel: false,
                        fontSize: 10,
                        interval: 0,  // 确保所有标签都显示
                       
                    },
                    data: ['0', 'I', 'II', 'III', 'IV'],
                    axisTick: {
                        alignWithLabel: true
                    },
                    // 新增：缩小类目间距
                    axisLabel: {
                        interval: 0,  // 设置为 0 表示所有标签都显示
                        rotate: 0,    // 标签不旋转
                    },
                    axisTick: {
                        alignWithLabel: true
                    },
                    axisLine: {
                        show: true
                    },
                    categoryGap: '5%'  // 缩小类目之间的间距
                }
            ],
            yAxis: [
                {
                    type: 'value',
                    interval: 50,
                    min: 0,
                    max: 400,
                    splitNumber: 7,
                    axisLine: {
                        symbol: ['none', 'arrow'],
                        symbolSize: [6, 6],
                        lineStyle: {
                            color: '#122C49'
                        }
                    },
                    axisLabel: {
                        color: '#61B9C8',
                        showMaxLabel: false,
                        fontSize: 10
                    },
                    splitLine: {
                        show: false,
                    },
                    name: '(次)',
                    nameGap: -10,
                    nameTextStyle: {
                        color: '#61B9C8',
                        fontSize: 9,
                        align: 'right',
                        padding: [0, 6, 0, 0]
                    }
                },
                {
                    type: 'value',
                    interval: 50,
                    position: "right",
                    offset: -35,
                    min: 0,
                    max: 400,
                    splitNumber: 7,
                    axisLine: {
                        symbol: ['none', 'arrow'],
                        symbolSize: [6, 6],
                        lineStyle: {
                            color: '#122C49'
                        }
                    },
                    axisLabel: {
                        color: '#61B9C8',
                        showMaxLabel: false,
                        fontSize: 10
                    },
                    splitLine: {
                        show: false,
                    },
                    name: '(次)',
                    nameGap: -10,
                    nameTextStyle: {
                        color: '#61B9C8',
                        fontSize: 9,
                        align: 'left',
                        padding: [0, 0, 0, 6]
                    }
                },
                {
                    type: 'value',
                    position: "right",
                    interval: 50,
                    min: 0,
                    max: 400,
                    splitNumber: 7,
                    axisLine: {
                        symbol: ['none', 'arrow'],
                        symbolSize: [6, 6],
                        lineStyle: {
                            color: '#122C49'
                        }
                    },
                    axisLabel: {
                        color: '#61B9C8',
                        showMaxLabel: false,
                        fontSize: 10
                    },
                    splitLine: {
                        show: false,
                    },
                    name: '(次)',
                    nameGap: -10,
                    nameTextStyle: {
                        color: '#61B9C8',
                        fontSize: 9,
                        align: 'left',
                        padding: [0, 0, 0, 6]
                    }
                }
            ],
            series: [
                {
                    name: '生产量',
                    type: 'bar',
                    barGap: 0,
                    barWidth: 25,
                    data: [200, 49, 70, 232, 256],  // 根据新的横坐标，调整数据数组的长度
                    itemStyle: {
                        borderRadius: [3, 3, 0, 0],
                        color: {
                            type: 'linear',
                            x: 0,
                            y: 0,
                            x2: 1,
                            y2: 0,
                            colorStops: [
                                {
                                    offset: 0, color: 'rgba(252,145,134,1)' // 0% 处的颜色
                                }, {
                                    offset: 1, color: 'rgba(241,88,135,1)' // 100% 处的颜色
                                }
                            ],
                            global: false // 缺省为 false
                        } //背景渐变色
                    }
                },
                {
                    name: '下载量',
                    type: 'bar',
                    barGap: 0,
                    barWidth:25,
                    data: [26, 59, 90, 264, 287],  // 根据新的横坐标，调整数据数组的长度
                    itemStyle: {
                        borderRadius: [3, 3, 0, 0],
                        color: {
                            type: 'linear',
                            x: 0,
                            y: 0,
                            x2: 1,
                            y2: 0,
                            colorStops: [
                                {
                                    offset: 0, color: 'rgba(144,20,238,1)' // 0% 处的颜色
                                }, {
                                    offset: 1, color: 'rgba(74,8,211,1)' // 100% 处的颜色
                                }
                            ],
                            global: false // 缺省为 false
                        } //背景渐变色
                    }
                },
                {
                    name: '访问量',
                    type: 'bar',
                    barGap: 0,
                    barWidth: 25,
                    data: [264, 287, 150, 175.6, 182.2],  // 根据新的横坐标，调整数据数组的长度
                    itemStyle: {
                        borderRadius: [3, 3, 0, 0],
                        color: {
                            type: 'linear',
                            x: 0,
                            y: 0,
                            x2: 1,
                            y2: 0,
                            colorStops: [
                                {
                                    offset: 0, color: 'rgba(0,204,255,1)' // 0% 处的颜色
                                }, {
                                    offset: 1, color: 'rgba(8,59,126,1)' // 100% 处的颜色
                                }
                            ],
                            global: false // 缺省为 false
                        } //背景渐变色
                    }
                }
            ]
        };
        myChart.setOption(option);
    },
}
</script>

<style scoped>
.threeBarChart {
    height: 100%;
    width: 100%;
}
</style>
