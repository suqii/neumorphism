<template>
  <div class="con" :style="mainStyle">
    <div class="demo-color-block">
      <span class="demonstration" :style="txtStyle">背景色</span>
      <el-color-picker @change="changeBgColor" v-model="color1" />
      <el-switch
        v-model="together"
        size="large"
        active-text="Open"
        inactive-text="宽高同步"
      />
    </div>

    <div class="slider-demo-block">
      <span class="demonstration" :style="txtStyle">高度</span>
      <el-slider
        :min="10"
        :max="400"
        @input="changeH"
        v-model="HValue"
      ></el-slider>
    </div>
    <div class="slider-demo-block">
      <span class="demonstration" :style="txtStyle">宽度</span>
      <el-slider
        :min="10"
        :max="400"
        @input="changeW"
        v-model="WValue"
      ></el-slider>
    </div>

    <div class="slider-demo-block">
      <span class="demonstration" :style="txtStyle">radius</span>
      <el-slider
        :min="10"
        :max="WValue / 2"
        @input="changeRadius"
        v-model="radiusV"
      ></el-slider>
    </div>
    <!-- intensity -->
    <div class="slider-demo-block">
      <span class="demonstration" :style="txtStyle">Intensity</span>
      <el-slider
        :min="0"
        :max="0.6"
        :step="0.01"
        @input="changeIntensity"
        v-model="intensityV"
      ></el-slider>
    </div>
    <!-- blurV -->
    <div class="slider-demo-block">
      <span class="demonstration" :style="txtStyle">Blur</span>
      <el-slider
        :min="0"
        :max="100"
        @input="shaowChange"
        v-model="blurV"
      ></el-slider>
    </div>
    <!-- distance -->
    <div class="slider-demo-block">
      <span class="demonstration" :style="txtStyle">distance</span>
      <el-slider
        :min="0"
        :max="50"
        @input="shaowChange"
        v-model="distanceV"
      ></el-slider>
    </div>
    <!-- shape -->
    <div class="shape">
      <el-button type="primary" @click="activeLightSource = 1" circle>
        <i-mdi:account-box-multiple />
      </el-button>
      <el-button type="primary" @click="activeLightSource = 2" circle>
        <i-mdi:account-box-multiple />
      </el-button>
      <el-button type="primary" @click="activeLightSource = 3" circle>
        <i-mdi:account-box-multiple />
      </el-button>
      <el-button type="primary" @click="activeLightSource = 4" circle>
        <i-mdi:account-box-multiple />
      </el-button>
    </div>
    <!-- 盒子区 -->
    <div class="box">
      <div class="mainBox" :style="boxStyle"></div>
    </div>
    <!-- 代码高亮区 -->
    <div class="code">
      <!-- height:{{ HValue }}px; width:{{ WValue }}px; background:{{ color1 }};
      border-radius: {{ radiusV }}; box-shadow:{{ boxStyle['box-shadow'] }} -->

      <!-- <pre v-highlight><code class="css">
        height:{{ HValue }}px;
        height:{{ HValue }}px;
        width:{{ WValue }}px;
        background:{{ color1 }};
        border-radius: {{ radiusV }}px;
        box-shadow:{{ boxStyle['box-shadow'] }}
        </code></pre> -->
    </div>
  </div>
</template>

<script lang="ts" setup>
import { ref } from 'vue'

const color1 = ref('#E0E0E0')
const txtColor = ref('red')
const together = ref(true)
const HValue = ref(150)
const WValue = ref(150)
const radiusV = ref(5)
const intensityV = ref(0.15)
const blurV = ref(60)
const distanceV = ref(20)
const activeLightSource = ref(1)

watch(HValue, (newVal, oldValue) => {
  if (together.value) {
    WValue.value = newVal
    boxStyle.value.width = `${newVal}px`
  }
})
watch(WValue, (newVal, oldValue) => {
  if (together.value) {
    HValue.value = newVal
    boxStyle.value.height = `${newVal}px`
  }
})

// 样式缩写
const bRadius = 'border-radius'
watch(activeLightSource, () => {
  shaowChange()
})
let boxStyle = ref({
  width: `${HValue.value}px`,
  height: `${WValue.value}px`,
  // bRadius:`${radiusV.value}%`,
  background: '#E0E0E0',
  'box-shadow': '16px 16px 32px #2e5643,-16px -16px 32px #a6fff3',
})
let txtStyle = ref({
  color: `#001f3f`,
})
let mainStyle = ref({
  background: `${color1.value}`,
})
// 改变高度
const changeH = (val: number) => {
  boxStyle.value.height = `${val}px`
}
// 改变宽度
const changeW = (val: number) => {
  boxStyle.value.width = `${val}px`
}
// 背景颜色改变
const changeBgColor = () => {
  mainStyle.value.background = `${color1.value}`
  boxStyle.value.background = `${color1.value}`
  // 更新阴影
  shaowChange()
}
// 背景颜色改变
const changeIntensity = () => {
  shaowChange()

  // console.log(boxStyle.value)
}
// changeRadius改变
const changeRadius = () => {
  let bRadius = {
    'border-radius': `${radiusV.value}%`,
  }
  boxStyle.value = { ...boxStyle.value, ...bRadius }
}
const colorLuminance = (hex: string, lum: number) => {
  // validate hex string
  hex = String(hex).replace(/[^0-9a-f]/gi, '')
  if (hex.length < 6) {
    hex = hex[0] + hex[0] + hex[1] + hex[1] + hex[2] + hex[2]
  }
  lum = lum || 0

  // convert to decimal and change luminosity
  let rgb = '#',
    c,
    i
  for (i = 0; i < 3; i++) {
    c = parseInt(hex.substr(i * 2, 2), 16)
    c = Math.round(Math.min(Math.max(0, c + c * lum), 255)).toString(16)
    rgb += ('00' + c).substr(c.length)
  }

  return rgb
}
// 改变阴影
const shaowChange = () => {
  let positionX = 1
  let positionY = 1
  let angle = 1
  switch (activeLightSource.value) {
    case 1:
      positionX = distanceV.value
      positionY = distanceV.value
      angle = 145
      break
    case 2:
      positionX = distanceV.value * -1
      positionY = distanceV.value
      angle = 225
      break
    case 3:
      positionX = distanceV.value * -1
      positionY = distanceV.value * -1
      angle = 315
      break
    case 4:
      positionX = distanceV.value
      positionY = distanceV.value * -1
      angle = 45
      break
    default:
      positionX = distanceV.value
      positionY = distanceV.value
      angle = 145
      break
  }
  const darkColor = colorLuminance(color1.value, intensityV.value * -1)
  const lightColor = colorLuminance(color1.value, intensityV.value)
  const boxShadowPosition = activeLightSource.value === 4 ? 'inset' : ''
  const firstBoxShadow = `${boxShadowPosition} ${positionX}px ${positionY}px ${blurV.value}px ${darkColor}`
  const secondBoxShadow = `${boxShadowPosition} ${positionX * -1}px ${
    positionY * -1
  }px ${blurV.value}px ${lightColor}`
  let BoxShadow = `${firstBoxShadow},${secondBoxShadow}`
  let bShadow = {
    'box-shadow': BoxShadow,
  }
  boxStyle.value = { ...boxStyle.value, ...bShadow }
  // 初始化文本颜色
  txtStyle.value.color = getContrast(color1.value)
}
// 改变文本颜色
const getContrast = (hex) => {
  const r = parseInt(hex.substr(1, 2), 16),
    g = parseInt(hex.substr(3, 2), 16),
    b = parseInt(hex.substr(5, 2), 16),
    yiq = (r * 299 + g * 587 + b * 114) / 1000
  return yiq >= 128 ? '#001f3f' : '#F6F5F7'
}
// 初始化阴影
shaowChange()
</script>
<style scoped>
.con {
  height: 100vh;
  /* background: #000; */
}
.slider-demo-block {
  width: 60%;
  display: flex;
  align-items: center;
  /* background: #000; */
}
.slider-demo-block .el-slider {
  margin-top: 0;
  margin-left: 12px;
}
.slider-demo-block .demonstration {
  font-size: 18px;
  /* color: #001f3f; */
  /* color: var(--el-text-color-secondary); */
  line-height: 44px;
  flex: 1;
  overflow: hidden;
  text-overflow: ellipsis;
  white-space: nowrap;
  margin-bottom: 0;
}
.slider-demo-block .demonstration + .el-slider {
  flex: 0 0 70%;
}
.box {
  height: 400px;
  /* background: #000; */
  display: flex;
  justify-content: center;
  align-items: center;
}
/* 颜色选择器 */
.demo-color-block {
  display: flex;
  align-items: center;
  margin-bottom: 16px;
}
.demo-color-block .demonstration {
  margin-right: 16px;
}
.code {
  /* margin: 20% 0; */
}
/* 滑条样式重写 */
</style>
<style scoped>
:deep(.el-slider__button),
:deep(.el-slider__bar),
:deep(.el-slider__runway) {
  background: #001f3f;
  border: #001f3f;
}
</style>
