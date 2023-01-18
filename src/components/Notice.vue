<script>
import { ref } from "vue";
let id = 1;
export default {
  setup() {
    const collActName = ref([2]);
    const items = [
      {
        id: id++,
        title: "无法下载？",
        content:
          "请将完整的URL，请求头（headers），post数据（data）复制到文本框中。<br />headers：在你要下载的文件的下载页面按F12，在弹出的开发者工具中按顶栏的Network/网络，然后单击下载按钮，这时，Network/网络中就会多出一个请求，双击目标请求，就可以看到headers(请求标头，request headers),和post数据(查询字符/payload)",
      },
      {
        id: id++,
        title: "帮助UP主",
        content:
          '<p>UP主的主页: <a target="_blank" href="https://space.bilibili.com/480317855">https://space.bilibili.com/480317855</a></p>' +
          "<p>帮助到您或者宣传片合您胃口的话可以多多充电支持哦，虽然视频不好康，但也请三联加关注(doge)</p>",
      },
      {
        id: id++,
        title: "反馈",
        content: "QQ: 2260388415<br />Email: misaka@nekoit.org",
      },
      {
        id: id++,
        title: "免责声明",
        content:
          "1.本站不提供任何资源，仅作加速下载之用。<br />2.本站仅供学习或学术方面使用，使用本站产生的一切责任由使用者承担，与本站无关。（使用本站即表示同意以上观点）",
      },
    ];
    return {
      collActName,
      items,
    };
  },
};
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
