<template>
  <div class="search_box">
    <div class="search_header">
      <div class="img_box">
        <img :src="songListImg" alt="" />
      </div>
      <div>歌单</div>
      <h1 class="keyFont ">
        {{ songListName }}
      </h1>
      <!-- <span>{{ total }}个结果</span> -->
    </div>
    <div class="search_main">
      <el-scrollbar>
        <Table :table-data="songDetailList" />
      </el-scrollbar>
    </div>
  </div>
</template>

<script setup lang="ts">
import Table from '@/components/Table.vue'
import { getSongDeatilList, SongListDetail } from '@/api/getDetailUrl'
import { useRoute } from 'vue-router'
import { onMounted, ref } from 'vue'
import dayjs from 'dayjs'

const route = useRoute()
const songListName = ref<string>()
const songDetailList = ref<Array<any>>()
const songListImg = ref<string>()
onMounted(() => {
  getSongDetail()
  getDetail()
})

const getSongDetail = async () => {
  const res = (await SongListDetail(route.params.id)) as any
  songListName.value = res.playlist.name
  songListImg.value = res.playlist.coverImgUrl
}
const getDetail = async () => {
  const res = (await getSongDeatilList(route.params.id, 10, 0)) as any
  // console.log(res.songs)
  // songDetailList.value = res.songs
  songDetailList.value = res.songs.map((item) => {
    return {
      id: item.id,
      name: item.name,
      al: item.al,
      ar: item.ar,
      publishTime: dayjs(item.dt).format('mm:ss')
    }
  })
}
</script>
<style scoped lang="less">
.search_box {
  height: 100%;
  .search_main {
    height: 75%;
    margin-right: 10px;
  }
}
.keyFont{
  color: #470747;
}
.search_header {
  display: flex;
  align-items: center;
  margin-left: 10px;
  margin-bottom: 10px;
  .img_box {
    max-width: 100px;
    max-height: 100px;
    border: 0;
    img {
      width: 100%;
      height: 100%;
      border-radius: 50%;
    }
  }
  div {
    font-size: 12px;
    color: red;
    padding: 1px;
    border: 1px solid red;
    border-radius: 5px;
    margin-right: 5px;
  }
}
</style>
