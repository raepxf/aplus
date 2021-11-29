<template>
    <div>
        <van-tabbar v-model="active">
  <van-tabbar-item icon="home-o">首页</van-tabbar-item>
  <van-tabbar-item icon="location-o">发现</van-tabbar-item>
  <van-tabbar-item icon="eye-o">生活</van-tabbar-item>
  <van-tabbar-item icon="user-o">我的</van-tabbar-item>
</van-tabbar>
    </div>
</template>

<script lang="ts">
import { defineComponent, ref } from 'vue'

export default defineComponent({
    setup() {
      const active = ref(0);
    return { active }
    },
})
</script>

