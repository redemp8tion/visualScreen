<template>
  <div>
    <div>【关键词条】</div>
    <div ref="target" class="w-full h-full"></div>
  </div>
</template>

<script setup>
import { onMounted, ref, watch } from "vue";
import * as echarts from "echarts";
import "echarts-wordcloud";

const props = defineProps({
  data: {
    type: Object,
    required: true,
  },
});

const target = ref(null);
let myChart = null;

onMounted(() => {
  myChart = echarts.init(target.value);
  // 渲染echarts
  renderChart();
});

const randomRGB = () => {
  const r = Math.floor(Math.random() * 255);
  const g = Math.floor(Math.random() * 255);
  const b = Math.floor(Math.random() * 255);
  return `rgb(${r}, ${g}, ${b})`;
};

const renderChart = () => {
  const options = {
    series: [
      {
        // 类型
        type: "wordCloud",
        // 数据映射文本的大小范围
        sizeRange: [8, 46],
        // 文字旋转范围
        rotationRange: [0, 0],
        // 单词之间的间距
        gridSize: 0,
        // 渲染动画
        layoutAnimation: true,
        // 字体
        textStyle: {
          // 随机色值
          color: randomRGB,
        },
        // 高亮字体
        emphasis: {
          textStyle: {
            fontWeight: "bold",
            color: "#000",
          },
        },
        // 数据
        data: props.data.datas,
      },
    ],
  };
  myChart.setOption(options);
};

// 监听数据变化，重新渲染图表
watch(
  () => props.data,
  () => {
    renderChart();
  }
);
</script>

<style lang="scss" scoped></style>
