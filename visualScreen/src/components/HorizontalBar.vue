<template>
  <div>
    <div>【大区数据信息】</div>
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
      left: 0,
      right: 0,
      bottom: 0,
      top: 0,
      containLabel: true,
    },
    xAxis: {
      show: false,
      type: "value",
      // 最大值（防止触顶）
      max: function (value) {
        // 取整
        return parseInt(value.max * 1.2);
      },
    },
    yAxis: {
      type: "category",
      data: props.data.regions.map((item) => item.name),
      inverse: true,
      axisTick: {
        show: false,
      },
      axisLine: {
        show: false,
      },
      // 文字色值
      axisLabel: {
        color: "#9EB1C8",
      },
    },
    series: [
      {
        type: "bar",
        data: props.data.regions.map((item) => ({
          name: item.name,
          value: item.value,
        })),
        showBackground: true,
        backgroundStyle: {
          color: "rgba(180, 180, 180, 0.2)",
        },
        itemStyle: {
          color: "#479AD3", // 设置柱子的颜色
          borderRadius: 5, // 设置柱子的圆角
          shadowColor: "rgba(0, 0, 0, 0.3)", // 设置柱子的阴影颜色
          shadowBlur: 5, // 设置柱子的阴影模糊大小
        },
        barWidth: 12,
        label:{
            show:true,
            position: 'right',
            textStyle: {
                color: '#fff'
            }
        }
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
