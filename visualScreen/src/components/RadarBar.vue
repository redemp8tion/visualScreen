<template>
  <div>
    <div>【云端报警风险】</div>
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
    radar: {
      // 坐标系名
      name: {
        textStyle: {
          color: "#05D5FF",
          fontSize: 14,
        },
      },
      shape: "polygon",
      center: ["50%", "50%"],
      startAngle: 120,
      radius: "80%",
      // 轴线配置
      axisLine: {
        lineStyle: {
          color: "rgba(5, 213, 255, .8)",
        },
      },
      // 网格线配置
      splitLine: {
        show: true,
        lineStyle: {
          width: 1,
          color: "rgba(5, 213, 255, .8)", // 设置网格的颜色
        },
      },
      splitArea: {
        show: false,
      },
      // 坐标角度
      angleAxis: {
        // 坐标轴刻度最小值
        min: 0,
        // 坐标轴分割间隔
        interval: 5,
        // 刻度增长逆时针
        clockwise: false,
        // 不显示坐标轴刻度
        axisTick: {
          show: false,
        },
        // 不显示坐标轴文字
        axisLabel: {
          show: false,
        },
        // 不显示坐标轴线
        axisLine: {
          show: false,
        },
        // 不显示分割线
        splitLine: {
          show: false,
        },
      },
      // 径向轴
      radiusAxis: {
        // 最小值
        min: 0,
        // 间隔
        interval: 20,
        // 不显示分割线
        splitLine: {
          show: true,
        },
      },
      indicator: props.data.risks.map((item) => ({
        name: item.name,
        max: 100,
      })),
    },
    series: [
      {
        type: "radar",
        // 拐点的样式，还可以取值'rect','angle'等
        symbol: "circle",
        // 拐点的大小
        symbolSize: 10,
        // 折线拐点标志的样式
        itemStyle: {
          normal: {
            color: "#05D5FF",
          },
        },
        // 区域填充样式
        areaStyle: {
          normal: {
            color: "#05D5FF",
            opacity: 0.5,
          },
        },
        // 线条样式
        lineStyle: {
          width: 2,
          color: "#05D5FF",
        },
        // 图形上的文本标签
        label: {
          normal: {
            show: true,
            formatter: (params) => {
              return params.value;
            },
            color: "#fff",
          },
        },
        data: [
            {
                value: props.data.risks.map((item) => item.value)
            }
        ],
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
