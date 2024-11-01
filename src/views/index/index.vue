<script setup lang="ts">
import { ref } from "vue";
import ItemWrap from "@/components/item-wrap";
import LeftTop from "./left-top.vue";
import LeftCenter from "./left-center.vue";
import LeftBottom from "./left-bottom.vue";
import CenterMap from "./center-map.vue";
import CenterBottom from "./center-bottom.vue";
import RightTop from "./right-top.vue";
import RightCenter from "./right-center.vue";
import RightBottom from "./right-bottom.vue";

let selectData = ref([
  {
    value: "Option1",
    label: "液化天然气",
  },
  {
    value: "Option2",
    label: "液化氢气",
  },
  {
    value: "Option3",
    label: "甲醇",
  },
]);
let selectValue = ref("Option1");
let dateValue = ref(new Date());
let inputValue = ref("");
</script>

<template>
  <div class="index-box">
    <div class="contetn_left" style="width: 100%">
      <!-- <div class="pagetab">
        <div class="item">实时监测</div>
        <div class="item">统计分析</div>
      </div> -->
      <ItemWrap
        style="width: 25%"
        class="contetn_left-top contetn_lr-item"
        title="深圳关区液化天然气进出口情况展示"
      >
        <div class="secrch">
          <el-select
            v-model="selectValue"
            placeholder="Select"
            size="small"
            style="width: 100px; margin-right: 10px"
          >
            <el-option
              v-for="item in selectData"
              :key="item.value"
              :label="item.label"
              :value="item.value"
            />
          </el-select>
          <el-date-picker
            v-model="dateValue"
            type="date"
            placeholder="Pick a day"
            size="small"
            style="width: 100px"
          />
        </div>
        <!-- <LeftTop /> -->
        <RightTop />
      </ItemWrap>

      <ItemWrap
        style="width: 50%"
        class="contetn_left-top contetn_lr-item"
        title="船舶航迹"
      >
        <!-- <div class="mapSearch">
          <el-input
            v-model="inputValue"
            style="width: 120px"
            placeholder="输入IMO号搜索"
            clearable
            size="small"
          />
        </div> -->
        <!-- 世界地图 -->
        <RightCenter />
      </ItemWrap>

      <!-- <ItemWrap class="contetn_left-center contetn_lr-item" title="深圳关区液化氢气进出口情况展示"> -->
      <!-- <LeftCenter /> -->
      <!-- </ItemWrap> -->
      <ItemWrap
        style="width: 25%"
        class="contetn_left-bottom contetn_lr-item"
        title="绿色能源罐存数据情况展示"
      >
        <LeftTop />
      </ItemWrap>
    </div>
    <!-- <div class="contetn_center">
      <CenterMap class="contetn_center_top" title=" 船舶航迹" />
    </div> -->
    <div class="contetn_right" style="width: 100%">
      <ItemWrap
        style="width: 50%"
        class="contetn_left-bottom contetn_lr-item"
        title="船舶信息展示"
      >
        <LeftBottom />
      </ItemWrap>

      <ItemWrap
        style="width: 50%"
        class="contetn_left-bottom contetn_lr-item"
        title="船期计划展示"
      >
        <RightBottom />
      </ItemWrap>
    </div>
  </div>
</template>

<style scoped lang="scss">
.index-box {
  width: 100%;
  display: flex;
  min-height: calc(100% - 64px);
  justify-content: space-between;
  flex-direction: column;
}
//左边 右边 结构一样
.contetn_left,
.contetn_right {
  display: flex;
  justify-content: space-between;
  position: relative;
  width: 540px;
  box-sizing: border-box;
  flex-shrink: 0;
}
.contetn_center {
  flex: 1;
  // margin: 0 26px;
  display: flex;
  flex-direction: column;
  justify-content: space-around;
  .contetn_center-bottom {
    height: 315px;
  }
}

.contetn_lr-item {
  position: relative;
  height: 500px;
}
.secrch {
  position: absolute;
  top: 43px;
  left: 13px;
  z-index: 9999999;
}
.mapSearch {
  position: absolute;
  top: 20px;
  right: 13px;
  z-index: 9999999;
}
</style>
