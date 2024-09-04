<template>
  <div class="static-table">
    <table>
      <tbody>
        <tr>
          <td v-for="(category, index) in categories" :key="index">
            <span class="category-label">{{ category.label }}</span>
            <span class="category-data">{{ category.data }}</span>
          </td>
        </tr>
      </tbody>
    </table>
  </div>
</template>

<script>
import axios from 'axios';

export default {
  name: "StaticDataTable",
  data() {
    return {
      categories: [], // 初始为空
    };
  },
  mounted() {
    this.fetchData(); // 组件挂载时请求数据
  },
  methods: {
    async fetchData() {
      try {
        const response = await axios.get('/majortext.json'); // 替换为实际路径
        this.categories = response.data.categories; // 更新组件数据
      } catch (error) {
        console.error('Error fetching data:', error);
      }
    },
  },
};
</script>

<style scoped>
table {
  width: 100%;
  border-collapse: collapse;
}

th,
td {
  padding: 2px;
  font-size: 20px; /* 调整字体大小 */
  text-align: center;
  transition: background-color 0.5s ease; /* 背景色的过渡效果 */
  font-family: "Arial Black", Gadget, sans-serif; /* 粗体和现代字体 */
}

.category-label {
  display: block;
  font-size: 1.2em;
  color: #20abe2; /* 番茄红色 */
  text-shadow: 2px 2px 8px rgba(191, 222, 12, 0.3); /* 添加阴影效果 */
  margin-bottom: 5px;
}

.category-data {
  font-size: 1.5em;
  color: #fdfdfd; /* 亮绿色 */
  font-weight: bold;
  text-shadow: 3px 3px 10px rgba(3, 135, 212, 0.5); /* 更明显的阴影 */
}
</style>
