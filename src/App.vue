<script setup>
import DownloaderVue from "./components/Downloader.vue";
import NoticeVue from "./components/Notice.vue";
</script>

<template>
  <el-menu background-color="#545c64" text-color="#fff">
    <el-menu-item index="0">文件下载加速器</el-menu-item>
    <div class="flex-grow" />
  </el-menu>
  <br />
  <el-main>
    <el-row>
      <el-col :span="11" :offset="0">
        <DownloaderVue />
      </el-col>
      <el-col :span="2"></el-col>
      <el-col :span="11" :offset="0">
        <NoticeVue />
      </el-col>
    </el-row>
  </el-main>
</template>
