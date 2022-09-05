<template>
  <div class="home-container">
    <van-nav-bar  title="老牛头条" fixed/>
    <van-pull-refresh v-model="isLoading" :disabled="finished" @refresh="onRefresh">
    <van-list v-model="loading" :finished="finished" finished-text="没有更多了" @load="onLoad">
    <ArticleInfo v-for="item in artList" :key="item.id" :title="item.title" :author="item.aut_name" :cmt-count="item.comm_count" :time="item.pubdate" :cover = 'item.cover'></ArticleInfo>
    </van-list>
    </van-pull-refresh>
  </div>
</template>

<script>
import { getArticleListApi } from '@/api/articleAPI.js'
import ArticleInfo from '@/components/Article/ArticleInfo.vue'
export default {
  name: 'Home',

  data () {
    return {
      page: 1,
      limit: 10,
      artList: [],
      // 是否正在加载下一页数据，如果 loading 为 true，则不会反复触发 load 事件
      // 每当下一页数据请求回来之后，千万要记得，把 loading 从 true 改为 false
      loading: true,
      // 所有数据是否加载完毕了，如果没有更多数据了，一定要把 finished 改成 true
      finished: false,
      // 是否正在下拉刷新
      isLoading: false

    }
  },
  created () {
    this.initArticleList()
  },
  methods: {
    async initArticleList (isRefresh) {
      const { data: res } = await getArticleListApi(this.page, this.limit)
      if (isRefresh) {
        this.artList = [...res, ...this.artList]
        this.isLoading = false
      } else {
        console.log(res)
        this.artList = [...this.artList, ...res]
        this.loading = false
      }

      if (res.length === 0) {
        this.finished = true
      }
    },
    onLoad () {
      this.page++
      this.initArticleList()
    },
    onRefresh () {
      this.page++
      this.initArticleList(true)
    }

  },
  components: {
    ArticleInfo

  }
}
</script>

<style lang="less" scoped>
  .home-container{
    padding: 46px 0  50px 0 ;

  }

</style>
