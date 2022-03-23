<template>
  <div class="flex border-b-2 border-gray-100">
    <div class="flex-1 h-16 bg-green-0 flex justify-end">
      <div
        class="bg-cover bg-center w-16 transform scale-75"
        style="
          background-image: url('https://suqiqi.oss-cn-beijing.aliyuncs.com/data/Logo/Gitee.png');
        "
      ></div>
      <div
        class="flex justify-center items-center font-semibold text-2xl italic"
      >
        Tailwindcss
      </div>
      <div class="w-20 flex justify-center items-center">标题3</div>
      <div class="w-20 flex justify-center items-center">标题3</div>
      <div class="w-20 flex justify-center items-center">标题4</div>
    </div>
    <div class="w-20 bg-red-0"></div>
    <div class="flex-1 h-16 bg-blue-0 flex justify-start">
      <div class="w-20 flex justify-center items-center">标题1</div>
      <div class="w-20 flex justify-center items-center">标题2</div>
      <div class="w-20 flex justify-center items-center">标题3</div>
      <div class="w-20 flex justify-center items-center">标题4</div>
    </div>
  </div>
  <router-view />
</template>
<style lang="less" scoped></style>
