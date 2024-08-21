<template>
  <div class="body">
    <div class="imageForm">
      <div v-if="imagePreview">
        <h3>当前选择的图片预览：</h3>
        <img :src="imagePreview" alt="Selected Image" style="max-width: 200px; height: auto;" />
      </div>
      <div class="response" v-if="responseData">
        <h3>Class: {{ responseData.class }}</h3>
        <h3>Confidence: {{ responseData.confidence }}</h3>
      </div>
    </div>
    <div class="footer">
      <el-upload class="upload-demo" action="" :show-file-list="false" :before-upload="handleBeforeUpload">
        <el-button size="default" type="primary">点击上传图片</el-button>
      </el-upload>
      <el-button  :disabled="!imagePreview" type="success" @click="uploadImage">上传并处理图片</el-button>
    </div>

  </div>

</template>

<script>
// 导入封装的请求方法
import { post } from '@/services/api';

export default {
  data() {
    return {
      selectedFile: null,
      imagePreview: null,
      processedImage: null,
      responseData: {
        class: "NULL",
        confidence: "NULL"

      }
    };
  },
  methods: {
    handleBeforeUpload(file) {
      this.selectedFile = file;

      const reader = new FileReader();
      reader.readAsDataURL(file);

      reader.onload = () => {
        this.imagePreview = reader.result;
      };

      return false; // 阻止自动上传
    },
    async uploadImage() {
      if (!this.selectedFile) return;

      const reader = new FileReader();
      reader.readAsDataURL(this.selectedFile);

      reader.onload = async () => {
        console.log(reader)
        const base64Image = reader.result;
        const cleanedBase64String = base64Image.replace(/^data:image\/\w+;base64,/, '');

        try {
          // 使用封装的 POST 方法
          const response = await post('/api/classify', { image: cleanedBase64String });
          // this.processedImage = response.processedImage; // 假设返回的处理结果在 `processedImage` 字段中
          response.confidence = (response.confidence * 100).toFixed(2) + '%';
          this.responseData = response
          console.log(response)
        } catch (error) {
          console.error('上传图片失败:', error);
        }
      };
    },
  },
};
</script>

<style>
.imageForm {
  display: flex;
  flex-direction: row;
  gap: 10px;
}

.response {
  width: 100%;
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
  margin-left: 20px;
  /* 使响应数据与图片之间有一些间距 */
}
.footer{
  margin-top: 20px;
  display: flex;
  align-items: center;
  justify-content: center;
  gap: 10px;

}
.upload-demo {
  display: flex;
  align-items: center;
  justify-content: center;
  margin-bottom: 0 !important;
}
.upload-demo .el-button{
  /* height: 100px; */
}

</style>