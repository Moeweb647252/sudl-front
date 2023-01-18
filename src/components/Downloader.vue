<script>
import { reactive, ref } from "vue";
import jsmd5 from "js-md5";

export default {
  setup() {
    const dlFormRef = ref();
    const dlForm = reactive({
      url: "",
      cookies: "",
      data: "",
      method: "GET",
      headers:
        "User-Agent: Mozilla/5.0 (X11; Linux x86_64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/109.0.0.0 Safari/537.36",
      dlserver: "//dls1.nekoit.org",
    });
    const salt = "salt";
    function getTimeStamp(){
      return Date.parse(new Date()).toString().slice(0,-3);
    }
    const dlservers = reactive({
      线路1: "//dls1.nekoit.org",
      "线路2(CloudFlare)": "//dls2.nekoit.org",
      线路3: "//dls3.nekoit.org",
      测试线路: "//127.0.0.1:5000",
    });

    const rules = reactive({
      url: [
        {
          required: true,
          message: "请输入下载地址",
        },
        {
          pattern: new RegExp(
            "(https?)://[-A-Za-z0-9+&@#/%?=~_|!:,.;]+[-A-Za-z0-9+&@#/%=~_|]"
          ),
          message: "请输入正确的下载地址",
        },
      ],
      method: [
        {
          required: true,
          message: "请选择请求方法",
        },
      ],
    });

    const onSubmit = async (formRef) => {
      await formRef.validate((valid, fields) => {
        if (valid) {
          const tmp = document.createElement("a");
          tmp.setAttribute("class", "display: none;");
          tmp.setAttribute(
            "href",
            dlForm.dlserver +
              "/download?url=" +
              dlForm.url +
              "&method=" +
              dlForm.method +
              "&data=" +
              dlForm.data.replace(/\n/g, "%0A") +
              "&cookies=" +
              dlForm.cookies +
              "&headers=" +
              dlForm.headers.replace(/\n/g, "%0A") +
              "&sign=" +
              jsmd5(
                salt +
                getTimeStamp().slice(0,-3)
              )
          );
          tmp.setAttribute("download", "download");
          tmp.setAttribute("target", "_blank");
          tmp.click();
        }
      });
    };

    return {
      dlForm,
      rules,
      onSubmit,
      dlFormRef,
      dlservers,
    };
  },
};
</script>

<template>
  <el-card class="box-card">
    <div class="card-header">
      <span>下载(大小限制20G)</span>
    </div>
    <hr />
    <el-form
      :model="dlForm"
      ref="dlFormRef"
      :rules="rules"
      label-width="80px"
      :inline="false"
      size="default"
    >
      <el-form-item label="下载地址" prop="url">
        <el-input v-model="dlForm.url"></el-input>
      </el-form-item>
      <el-form-item label="请求方法">
        <el-select v-model="dlForm.method">
          <el-option label="GET" value="GET" />
          <el-option label="POST" value="POST" />
        </el-select>
      </el-form-item>
      <el-form-item label="Cookies">
        <el-input v-model="dlForm.cookies"></el-input>
      </el-form-item>
      <el-form-item label="请求参数" v-if="dlForm.method === 'POST'">
        <el-input v-model="dlForm.data" type="textarea" />
      </el-form-item>
      <el-form-item label="请求头">
        <el-input v-model="dlForm.headers" type="textarea" />
      </el-form-item>
      <el-form-item label="线路" size="normal">
        <el-select v-model="dlForm.dlserver">
          <el-option
            v-for="(item, key) in dlservers"
            :key="item"
            :label="key"
            :value="item"
          />
        </el-select>
      </el-form-item>
      <el-form-item>
        <el-button type="primary" @click="onSubmit(dlFormRef)"
          >下载（点击即表示同意免责声明）</el-button
        >
      </el-form-item>
    </el-form>
  </el-card>
</template>
