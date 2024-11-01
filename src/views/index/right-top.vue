<script setup lang="ts">
import { ref, onMounted } from "vue";
import { alarmNum } from "@/api";
import { graphic } from "echarts/core";
import { ElMessage } from "element-plus";

const option = ref({});
const getData = () => {
  alarmNum()
    .then((res) => {
      console.log("右上--报警次数 ", res);
      if (res.success) {
        setOption(res.data.dateList, res.data.numList, res.data.numList2);
      } else {
        ElMessage({
          message: res.msg,
          type: "warning",
        });
      }
    })
    .catch((err) => {
      ElMessage.error(err);
    });
};
const setOption = async (xData: any[], yData: any[], yData2: any[]) => {
  option.value = {
    tooltip: {
      trigger: "item",

      triggerOn: "click",
      // axisPointer: {
      //   type: "shadow",
      //   textStyle: {
      //     color: "#fff",
      //   },
      // },
      formatter: function (params: any) {
        console.log("params", params);
        let { seriesName, value } = params;

        return `年份:   2010<br/>
          月份:    12月<br/>
          船舶船次:   ${value}<br/>
          进(出)口票数:  ${value}<br/>
          货物重量:  ${value}<br/>
           进(出)口货值:  ${value}<br/>
        `;
      },
    },
    grid: {
      borderWidth: 0,
      top: 110,
      bottom: 95,
      textStyle: {
        color: "#fff",
      },
    },
    legend: {
      top: "11%",
      textStyle: {
        color: "#90979c",
      },
      data: ["船舶船次", "进(出)口票数", "货物重量", "进(出)口货值"],
      selectedMode: 'single', // 一次只能选中一个
    },

    calculable: true,
    xAxis: [
      {
        type: "category",
        axisLine: {
          lineStyle: {
            color: "rgba(204,187,225,0.5)",
          },
        },
        splitLine: {
          show: false,
        },
        axisTick: {
          show: false,
        },
        data: [12, 1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11],
      },
    ],

    yAxis: [
      {
        name: "Nm³",
        type: "value",
        splitLine: {
          show: false,
        },
        axisTick: {
          show: false,
        },
        axisLine: {
          lineStyle: {
            color: "rgba(204,187,225,0.5)",
          },
        },
      },
    ],

    series: [
      {
        name: "船舶船次",
        type: "line",
        symbolSize: 10,
        symbol: "circle",
        itemStyle: {
          color: "#6f7de3",
        },

        data: [
          509, 917, 2455, 2610, 2719, 3033, 3044, 3085, 2708, 2809, 2117, 2000,
          1455, 1210, 719, 733, 944, 2285, 2208, 3372, 3936, 3693, 2962, 2810,
          3519, 2455, 2610, 2719, 2484, 2078,
        ],
      },
      {
        name: "进(出)口票数",
        type: "line",
        symbolSize: 10,
        symbol: "circle",
        itemStyle: {
          color: "#c257F6",
        },

        data: [
          2136, 3693, 2962, 3810, 3519, 3484, 3915, 3823, 3455, 4310, 4019,
          3433, 3544, 3885, 4208, 3372, 3484, 3915, 3748, 3675, 4009, 4433,
          3544, 3285, 4208, 3372, 3484, 3915, 3823, 4265, 4298,
        ],
      },
      {
        name: "货物重量",
        type: "line",
        symbolSize: 10,
        symbol: "circle",
        itemStyle: {
          color: "#c1f657",
        },

        data: [
          2136, 3693, 2962, 3810, 3519, 3484, 3915, 3823, 3455, 4310, 4019,
          3433, 3544, 3885, 4208, 3372, 3484, 3915, 3748, 3675, 4009, 4433,
          3544, 3285, 4208, 3372, 3484, 3915, 3823, 4265, 4298,
        ],
      },
      {
        name: "进(出)口货值",
        type: "line",
        symbolSize: 10,
        symbol: "circle",
        itemStyle: {
          color: "orange",
        },

        data: [
          2136, 3693, 2962, 3810, 3519, 3484, 3915, 3823, 3455, 4310, 4019,
          3433, 3544, 3885, 4208, 3372, 3484, 3915, 3748, 3675, 4009, 4433,
          3544, 3285, 4208, 3372, 3484, 3915, 3823, 4265, 4298,
        ],
      },
    ],
  };
};
onMounted(() => {
  getData();
});
</script>

<template>
  <v-chart
    class="chart"
    :option="option"
    v-if="JSON.stringify(option) != '{}'"
    autoresize
  />
  
</template>

<style scoped lang="scss"></style>
