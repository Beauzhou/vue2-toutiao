<template>
  <div>

    <!-- 路由占位 -->
    <router-view></router-view>

    <van-tabbar route>
    <van-tabbar-item replace to="/" icon="home-o">标签</van-tabbar-item>
    <van-tabbar-item replace to="/user" icon="user-o">标签</van-tabbar-item>
    </van-tabbar>

  </div>
</template>

<script>

export default {
  name: 'App'
}
</script>

<style>
</style>
