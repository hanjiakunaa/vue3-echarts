<script setup >
import { ref, nextTick, reactive, onMounted } from "vue";

import * as echarts from "echarts";
// 世界地图数据（更改后的JSON数据）
import world from "./world.json";
// 注册世界地图数据
echarts.registerMap("world", world);

// 初始化
let myChart = ref(null);

// 小飞机图标
let planePath = "path://M574.4 0L825.2 120.4c29.2 15.10000001 47.5 45.1 47.49999999 78L872.69999999 908c0 13.09999999-13.8 21.59999999-25.49999999 15.7l-0.6-0.29999999c-19.1-9.6-42.3-3.9-54.7 13.49999999l-0.7 1C762.4 978.1 715.9 1002.00000001 666.4 1002.00000001l-56.9 0 0-631.80000001-0.4 0c-19.2 0-34.7-15.6-34.69999999-34.7L574.4 0z,M363.8 405.4L363.8 528.2c0 9.7 7.9 17.6 17.49999999 17.6l193.00000001 0c9.7 0 17.5-7.9 17.49999999-17.6l0-122.8c0-9.7-7.9-17.5-17.49999999-17.5l-193.00000001 0c-9.7-0.1-17.5 7.8-17.49999999 17.5zM434 194.8L434 335.1c0 9.7 7.9 17.6 17.5 17.6l70.2 0c9.7 0 17.5-7.9 17.5-17.6l0-140.4c0-9.7-7.9-17.6-17.5-17.6l-70.2 0c-9.7 0.1-17.5 8-17.5 17.7z,M591.9 738.8l0-140.4c0-9.7-7.9-17.5-17.5-17.5L434 580.9c-9.7 0-17.6 7.9-17.60000001 17.5L416.4 738.8c0 9.7 7.9 17.6 17.6 17.6l140.4 0c9.7-0.1 17.5-7.9 17.5-17.6z,M333.7 874.7L284 874.70000001l0-33.20000001-33.2 1e-8 0 33.19999999-99.5 0 0 16.6 99.5 0L250.8 941.1 284 941.1l0-49.80000001 49.8 1e-8L333.8 1024 367 1024l0-49.8 232.2-1e-8 0-182.49999999L363.8 791.7l-30.1-44 0 127zM532.8 825l-1e-8 49.8-66.29999999 0L466.5 825.00000001l66.3-1e-8z m-99.6 0l0 49.8-49.80000001 0L383.4 825l49.8 0z"
// let planePath =
//   "path://M1024 574.4L903.6 825.2c-15.1 29.2-45.1 47.5-78 47.5H116c-13.1 0-21.6-13.8-15.7-25.5l0.3-0.6c9.6-19.1 3.9-42.3-13.5-54.7l-1-0.7C45.9 762.4 22 715.9 22 666.4v-56.9h631.8v-0.4c0-19.2 15.6-34.7 34.7-34.7H1024z,M618.6 363.8H495.8c-9.7 0-17.6 7.9-17.6 17.5v193c0 9.7 7.9 17.5 17.6 17.5h122.8c9.7 0 17.5-7.9 17.5-17.5v-193c0.1-9.7-7.8-17.5-17.5-17.5zM829.2 434H688.9c-9.7 0-17.6 7.9-17.6 17.5v70.2c0 9.7 7.9 17.5 17.6 17.5h140.4c9.7 0 17.6-7.9 17.6-17.5v-70.2c-0.1-9.7-8-17.5-17.7-17.5z,M285.2 591.9h140.4c9.7 0 17.5-7.9 17.5-17.5V434c0-9.7-7.9-17.6-17.5-17.6H285.2c-9.7 0-17.6 7.9-17.6 17.6v140.4c0.1 9.7 7.9 17.5 17.6 17.5z,M149.3 333.7V284h33.2v-33.2h-33.2v-99.5h-16.6v99.5H82.9V284h49.8v49.8H0V367h49.8v232.2h182.5V363.8l44-30.1h-127zM199 532.8h-49.8v-66.3H199v66.3z m0-99.6h-49.8v-49.8H199v49.8z";
// let planePath =  "path://M1705.06,1318.313v-89.254l-319.9-221.799l0.073-208.063c0.521-84.662-26.629-121.796-63.961-121.491c-37.332-0.305-64.482,36.829-63.961,121.491l0.073,208.063l-319.9,221.799v89.254l330.343-157.288l12.238,241.308l-134.449,92.931l0.531,42.034l175.125-42.917l175.125,42.917l0.531-42.034l-134.449-92.931l12.238-241.308L1705.06,1318.313z";
// 线路坐标（模拟数据）
let LineData = reactive([
  {
    fromName: "加拿大温哥华",
    toName: "咸阳国际机场",
    coords: [
      [-123.023921, 49.311753],
      [108.76463, 34.44212],
    ],
    value: 123,
    
  },
]);
// 线路起点坐标
let LineEffectScatterData = reactive([]);
// 线路终点坐标
let effectScatterData = reactive([]);
LineData.map((item) => {
  // 线路起点坐标

  LineEffectScatterData.push({ name: item.fromName, value: item.coords[0] });
  // 线路终点坐标

  effectScatterData.push({ name: item.toName, value: item.coords[1] });
});
let series = [];
series.push(
  {
    type: "lines",
    zlevel: 2,
    offset: [100,0],
    effect: {
      show: true,
      //飞机的速度  这里是s单位
      period: 100,
      trailLength: 0,
      symbol: planePath,
      // 飞机大小
      symbolSize: 35,
     
      color: "#ff8800",
      symbolRotate:function(val){
        return val.rotation || 180;
      },
    },
    lineStyle: {
      normal: {
        color: "#8A91BA",
        type: "dashed", // 虚线
        // 线条宽度
        width: 2,
        opacity: 1,
        curveness: 0.5, // 弯曲度
      },
      emphasis: {
        color: "#FF9600", //飞线悬浮颜色
      },
    },
    label: {
      normal: {
        show: false,
        position: "middle",
        formatter: "{b}",
      },
    },
    data: LineData,
  },
  {
    type: "effectScatter",
    coordinateSystem: "geo",
    zlevel: 2,
    rippleEffect: {
      //涟漪特效
      period: 4, //动画时间，值越小速度越快
      brushType: "stroke", //波纹绘制方式 stroke, fill
      scale: 4, //波纹圆环最大限制，值越大波纹越大
    },
    label: {
      normal: {
        show: false,
        position: "right", //显示位置
        offset: [5, 0], //偏移设置
        formatter: "{b}", //圆环显示文字
        textStyle: {
          color: "red",
        },
      },
      // emphasis: {
      //     show: true
      // }
    },
    symbol: "circle",
    symbolSize: function (val) {
      return 10; //圆环大小
    },
    itemStyle: {
      normal: {
        show: false,
        color: "#E0C896",
      },
    },
    data: LineEffectScatterData,
  },
  //终点
  {
    type: "effectScatter",
    coordinateSystem: "geo",
    zlevel: 2,
    rippleEffect: {
      period: 4,
      brushType: "stroke",
      scale: 4,
    },
    label: {
      normal: {
        show: false,
        position: "right", //显示位置
        offset: [5, 0], //偏移设置
        formatter: "", //圆环显示文字
      },
      emphasis: {
        show: true,
      },
    },
    symbol: "circle",
    symbolSize: function (val) {
      return 10; //圆环大小
    },
    itemStyle: {
      normal: {
        show: true,
        color: "#FF9300",
      },
    },
    data: effectScatterData,
  }
);

let option;
option = {
  backgroundColor: "rgba(0,0,0,0)",
  tooltip: {
    trigger: "item",
    backgroundColor: "rgba(255,255,255,0.8)",
    borderColor: "rgba(0,0,0,0)",
    padding: 20,
    formatter: function (params, ticket, callback) {
      if (params.seriesType == "effectScatter") {
        // 这里可根据自己需要自定义
        // 起点终点悬浮效果
        // return params.marker + params.data.name;
      } else if (params.seriesType == "lines") {
        // 这里可根据自己需要自定义
        // 飞线悬浮效果
        return `船名:   XXXX<br/>
          IMO号:    xxxxx<br/>
          航次号:   xxxx<br/>
          商品名称:  xxxx<br/>
          装载数量:  xxxx<br/>
           
        `;
      }
      return params.name;
    },
  },
  geo: {
    map: "world",
    label: {
      emphasis: {
        show: false,
      },
    },
    roam: true, //是否允许缩放
    scaleLimit: {
      //滚轮缩放的极限控制
      min: 1,
      max: 3,
    },
    layoutCenter: ["center", "center"], //地图位置
    layoutSize: "180%",
    itemStyle: {
      normal: {
        color: "#6073ae", //地图背景色
        borderColor: "#6073ae", //省市边界线
      },
      emphasis: {
        color: "#6073ae", //悬浮背景
      },
    },
    // 设置中国地图边界线
    regions: [
      {
        name: "China",
        itemStyle: {
          normal: {
            borderColor: "#fedc70",
            borderWidth: 1,
            areaColor: "#6073ae",
            // shadowColor: 'rgba(128, 217, 248, 1)',
            // shadowColor: 'rgba(255, 255, 255, 1)',
            shadowOffsetX: 0,
            shadowOffsetY: 0,
            shadowBlur: 0,
          },
          emphasis: {
            color: "#6073ae", //悬浮背景
          },
        },
      },
    ],
  },

  series: series,
};
nextTick(() => {
  let chartDom = document.getElementById("main");
  myChart = echarts.init(chartDom);
  myChart.setOption(option);
})

window.addEventListener("resize", () => {
  myChart.resize();
});
</script>

<template>
  <div class="main" id="main"></div>
</template>
<style scoped lang="scss">
.main {
  width: 912px;
  height: 100%;
  overflow: hidden;
}
</style>


