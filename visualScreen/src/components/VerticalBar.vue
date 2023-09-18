<template>
  <div>
    <div>【服务资源占用比】</div>
    <div ref="target" class="w-full h-full"></div>
  </div>
</template>

<script setup>
import { onMounted, ref, watch } from "vue";
import * as echarts from "echarts";

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

const renderChart = () => {
  const options = {
    grid: {
      top: 16,
      right: 0,
      bottom: 26,
      left: -26,
      containLabel: true,
    },
    xAxis: {
      type: "category",
      data: props.data.servers.map((item) => item.name),
      axisLabel: {
        color: "#9EB1C8",
      },
    },
    yAxis: {
      type: "value",
      show: false,
      max: function (value) {
        return parseInt(value.max * 1.2);
      },
    },

    series: {
      // 柱形图
      type: "bar",
      // 数据筛选
      data: props.data.servers.map((item) => ({
        name: item.name,
        value: item.value,
      })),
      // 每个轴的样式
      itemStyle: {
        color: "#479AD3", // 设置柱子的颜色
        barBorderRadius: 5, // 设置柱子的圆角
        shadowColor: "rgba(0, 0, 0, 0.3)", // 设置柱子的阴影颜色
        shadowBlur: 5, // 设置柱子的阴影模糊大小
      },
      // 柱子宽度
      barWidth: 12,
      // 文本
      label: {
        show: true,
        // 设置标签位置为右侧
        position: "top",
        textStyle: {
          // 设置标签文本颜色
          color: "#fff",
        },
        formatter: "{c}%",
      },
    },
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
