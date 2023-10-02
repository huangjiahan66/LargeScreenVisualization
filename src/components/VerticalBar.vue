<template>
  <div>
    <div>服务资源占用比</div>
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

// dom
const target = ref(null);
let mChart = null;
onMounted(() => {
  mChart = echarts.init(target.value);
  randerChart();
});

const randerChart = () => {
  const options = {
    xAxis: {
      type: "category",
      data: props.data.servers.map((item) => item.name),
      axisLabel: {
        color: "#9EB1C8",
      },
    },
    // 如果y轴展示数据  就是type value
    yAxis: {
      type: "value",
      show: false,
      max: function (value) {
        return parseInt(value.max * 1.2);
      },
    },
    grid: {
      top: 16,
      right: 0,
      bottom: 26,
      left: -26,
      containLabel: true,
    },
    series: [
      {
        // 图表类型
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
        // 轴宽度
        barWidth: 12,
        // 轴上的字体
        label: {
          show: true,
          // 设置标签位置为右侧
          position: "right",
          textStyle: {
            // 设置标签文本颜色
            color: "#fff",
          },
          position: "top",
          formatter: "{c}%",
        },
      },
    ],
  };
  mChart.setOption(options);
};

watch(() => props.data, randerChart);
</script>

<style lang="scss" scoped></style>
