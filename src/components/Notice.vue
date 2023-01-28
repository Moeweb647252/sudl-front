<script setup>
import { ref,onMounted } from "vue";
import axios from "axios";

const collActName = ref([1]);
const items = ref();

onMounted(()=>{
  axios.get("/notice.json").then((res) => {
    items.value = res.data;
  });
})

</script>

<template>
  <el-card class="box-card">
    <div class="card-header">
      <span>公告栏</span>
    </div>
    <hr />
    <el-collapse v-model="collActName" :accordion="false" @change="">
      <el-collapse-item
        v-for="item in items"
        :name="item.id"
        :title="item.title"
      >
        <div v-html="item.content"></div>
      </el-collapse-item>
    </el-collapse>
  </el-card>
</template>
