<script setup lang="ts">
import { ref, reactive } from "vue";
import { graphic } from "echarts/core";
import { countUserNum } from "@/api";
import { ElMessage } from "element-plus";

let colors = ["#0BFC7F", "#A0A0A0", "#F48C02", "#F4023C"];
const option = ref({});
const state = reactive({
  lockNum: 0,
  offlineNum: 0,
  onlineNum: 0,
  alarmNum: 0,
  totalNum: 0,
});
const echartsGraphic = (colors: string[]) => {
  return new graphic.LinearGradient(1, 0, 0, 0, [
    { offset: 0, color: colors[0] },
    { offset: 1, color: colors[1] },
  ]);
};
const getData = () => {
  countUserNum()
    .then((res) => {
      console.log("左中--用户总览", res);
      if (res.success) {
        // state.lockNum = res.data.lockNum;
        // state.offlineNum = res.data.offlineNum;
        // state.onlineNum = res.data.onlineNum;
        // state.totalNum = res.data.totalNum;
        // state.alarmNum = res.data.alarmNum;
        setOption();
      } else {
        ElMessage.error(res.msg);
      }
    })
    .catch((err) => {
      ElMessage.error(err);
    });
};
getData();
const setOption = () => {
  option.value = {
    // title: {
    //   text: "2019年销售水量和主营业务收入对比",
    //   textStyle: {
    //     align: "center",
    //     color: "#fff",
    //     fontSize: 20,
    //   },
    //   top: "3%",
    //   left: "10%",
    // },
    grid: {
      top: "15%",
      bottom: "15%", //也可设置left和right设置距离来控制图表的大小
    },
    tooltip: {
      trigger: "axis",
      axisPointer: {
        type: "shadow",
        label: {
          show: true,
        },
      },
    },
    // legend: {
    //   data: ["销售水量", "主营业务"],
    //   top: "5%",
    //   textStyle: {
    //     color: "#ffffff",
    //   },
    // },
    xAxis: {
      data: [
        "票数",
        "重量",
        "货值",
        "趋势",
       
      ],
      axisLine: {
        show: true, //隐藏X轴轴线
        lineStyle: {
          color: "#FFFFFF",
        },
      },
      axisTick: {
        show: true, //隐藏X轴刻度
      },
      axisLabel: {
        show: true,
        textStyle: {
          color: "#ffffff", //X轴文字颜色
        },
      },
    },
    yAxis: [
      {
        type: "value",
        // name: "亿元",
        nameTextStyle: {
          color: "#ebf8ac",
        },
        splitLine: {
          show: false,
        },
        axisTick: {
          show: true,
        },
        axisLine: {
          show: true,
          lineStyle: {
            color: "#FFFFFF",
          },
        },
        axisLabel: {
          show: true,
          textStyle: {
            color: "#ebf8ac",
          },
        },
      },
      {
        type: "value",
        // name: "同比",
        nameTextStyle: {
          color: "#ebf8ac",
        },
        position: "right",
        splitLine: {
          show: false,
        },
        axisTick: {
          show: false,
        },
        axisLine: {
          show: false,
        },
        axisLabel: {
          show: true,
          formatter: "{value} %", //右侧Y轴文字显示
          textStyle: {
            color: "#ebf8ac",
          },
        },
      },
      {
        type: "value",
        gridIndex: 0,
        min: 50,
        max: 100,
        splitNumber: 8,
        splitLine: {
          show: false,
        },
        axisLine: {
          show: false,
        },
        axisTick: {
          show: false,
        },
        axisLabel: {
          show: false,
        },
        splitArea: {
          show: true,
          areaStyle: {
            color: ["rgba(250,250,250,0.0)", "rgba(250,250,250,0.05)"],
          },
        },
      },
    ],
    series: [
      {
        name: "销售水量",
        type: "line",
        yAxisIndex: 1, //使用的 y 轴的 index，在单个图表实例中存在多个 y轴的时候有用
        smooth: true, //平滑曲线显示
        showAllSymbol: true, //显示所有图形。
        symbol: "circle", //标记的图形为实心圆
        symbolSize: 10, //标记的大小
        itemStyle: {
          //折线拐点标志的样式
          color: "#058cff",
        },
        lineStyle: {
          color: "#058cff",
        },
        areaStyle: {
          color: "rgba(5,140,255, 0.2)",
        },
        data: [4.2, 3.8, 4.8, 3.5, 2.9, 2.8, 3, 5],
      },
      {
        name: "主营业务",
        type: "bar",
        barWidth: 15,
        itemStyle: {
          // normal: {
          //   color: new echarts.graphic.LinearGradient(0, 0, 0, 1, [
          //     {
          //       offset: 0,
          //       color: "#00FFE3",
          //     },
          //     {
          //       offset: 1,
          //       color: "#4693EC",
          //     },
          //   ]),
          // },
        },
        data: [4.2, 3.8, 4.8, 3.5, 2.9, 2.8, 3, 5],
      },
    ],
  };
};
</script>

<template>
  <v-chart class="chart" :option="option" />
</template>

<style scoped lang="scss"></style>
