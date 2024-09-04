<template>
    <div class="satellite-status-container">
        <div class="card">
            <h3>卫星位置</h3>
            <p><strong>经度:</strong> {{ longitude }}°</p>
            <p><strong>纬度:</strong> {{ latitude }}°</p>
            <p><strong>海拔高度:</strong> {{ altitude }} km</p>
        </div>

        <div class="card">
            <h3>轨道参数</h3>
            <p><strong>倾角:</strong> {{ inclination }}°</p>
            <p><strong>周期:</strong> {{ period }} 分钟</p>
            <p><strong>半长轴:</strong> {{ semiMajorAxis }} km</p>
        </div>

        <div class="card">
            <h3>卫星状态</h3>
            <p><strong>电量:</strong> {{ batteryLevel }}%</p>
            <p><strong>温度:</strong> {{ temperature }}°C</p>
            <p><strong>姿态控制:</strong> {{ attitudeControl }}%</p>
            
        </div>
    </div>
</template>

<script>
export default {
    data() {
        return {
            longitude: 120.0,        // 示例数据
            latitude: 30.0,          // 示例数据
            altitude: 500,           // 示例数据
            inclination: 45,         // 示例数据
            period: 95,              // 示例数据
            semiMajorAxis: 7000,     // 示例数据
            batteryLevel: 80,        // 示例数据
            temperature: 22,         // 示例数据
            attitudeControl: 90,     // 示例数据
            
        };
    }
};
</script>

<style scoped>
.satellite-status-container {
    display: flex;
    flex-wrap: wrap;
    gap: 15px;
    padding: 20px;
    /* 背景透明 */
    background-color: transparent;
}

.card {
    /* 设置背景透明并保留边框和阴影 */
    background-color: rgba(44, 147, 216, 0.5);
    border-radius: 10px;
    box-shadow: 0 2px 8px rgba(183, 12, 52, 0.1);
    padding: 20px;
    width: calc(33.3333% - 20px);
    height: 130px; 
    box-sizing: border-box;
}

.card h3 {
    margin-bottom: 15px;
    color: #170857;
    font-size: 20px;      /* 字体大小 */
    font-weight: bold;    /* 字体粗细 */
    font-family: 'Trebuchet MS', sans-serif;

}

.card p {
    margin: 2px 0;
    color: #c9da0b;
    font-size: 10px;      /* 字体大小 */
    font-weight: normal;  /* 字体粗细 */
    font-family: 'Trebuchet MS', sans-serif;
}

.card strong {
    color: #0e023b;
}
</style>
