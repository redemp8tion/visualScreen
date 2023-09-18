<template>
  <div>
    <div>【大区异常处理】</div>
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

const getSeriesData = () => {
  const series = [];

  props.data.abnormals.forEach((item, index) => {
    // 上层环形绘制
    series.push({
      name: item.name,
      type: "pie",
      clockWise: false,
      hoverAnimation: false,
      radius: [73 - index * 15 + "%", 68 - index * 15 + "%"],
      center: ["55%", "55%"],
      label: { show: false },
      data: [
        { value: item.value, name: item.name },
        {
          value: 1000,
          name: "",
          itemStyle: { color: "rgba(0,0,0,0)", borderWidth: 0 },
          tooltip: { show: false },
          hoverAnimation: false,
        },
      ],
    });

    // 底层图
    series.push({
      name: item.name,
      type: "pie",
      silent: true,
      z: 1,
      clockWise: false,
      hoverAnimation: false,
      radius: [73 - index * 15 + "%", 68 - index * 15 + "%"],
      center: ["55%", "55%"],
      label: { show: false },
      data: [
        {
          value: 7.5,
          itemStyle: { color: "rgb(3, 31, 62)", borderWidth: 0 },
          tooltip: { show: false },
          hoverAnimation: false,
        },
        {
          value: 2.5,
          name: "",
          itemStyle: { color: "rgba(0,0,0,0)", borderWidth: 0 },
          tooltip: { show: false },
          hoverAnimation: false,
        },
      ],
    });
  });
  return series;
};

const renderChart = () => {
  const options = {
    legend: {
      show: true,
      icon: "circle",
      top: "14%",
      left: "60%",
      data: props.data.abnormals.map((item) => item.name),
      width: -5,
      itemWidth: 10,
      itemHeight: 10,
      itemGap: 6,
      textStyle: {
        fontSize: 12,
        lineHeight: 5,
        color: "#ffffff",
      },
    },
    tooltip: {
      show: true,
      trigger: "item",
      formatter: "{a}<br>{b}:{c}({d}%)",
    },
    xAxis: {
      show: false,
    },
    yAxis: {
      type: "category",
      inverse: true,
      axisLine: {
        show: false,
      },
      axisTick: {
        show: false,
      },
    },
    series: getSeriesData(),
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
