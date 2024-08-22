<template>
  <div class="main">

    <div class="left">
      <div v-if="imagePreview">
        <h3>当前选择的图片预览：</h3>
        <img :src="imagePreview" alt="Selected Image" style="max-width: 500px; height: auto;" />
      </div>
      <div class="leftBottom">
        <el-upload class="upload-demo" action="" :show-file-list="false" :before-upload="handleBeforeUpload">
          <el-button size="default" type="primary">点击上传图片</el-button>
        </el-upload>
        <el-button v-if="imagePreview" type="success" @click="uploadImage">上传并处理图片</el-button>
      </div>

    </div>
    <div class="right">
      <div v-if="processedImage">
        <h3>处理后的图片：</h3>
        <img :src="processedImage" alt="Processed Image" style="max-width: 500px; height: auto;" />
      </div>
    </div>


    <!-- <div v-if="responseData">
        <h3>Class: {{ responseData.class }}</h3>
        <h3>Confidence: {{  responseData.confidence }}</h3>
      </div> -->
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
      responseData: null
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
          const response = await post('/api/segmentate', { image: cleanedBase64String });
          // this.processedImage = response.processedImage; // 假设返回的处理结果在 `processedImage` 字段中
          // response.confidence = (response.confidence * 100).toFixed(2) + '%';
          this.processedImage = "data:image/jpeg;base64," + response.segmented_image
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
* {
  padding: 0;
  margin: 0;
}

.main {
  height: 100%;
  display: flex;
  flex-direction: row;
  justify-content: center;
  gap: 10%;
}

.left {
  display: flex;
  flex-direction: column;
  gap: 10%;
  justify-content: center;
  align-items: center;
}

.leftBottom {
  display: flex;
  justify-content: center;
  align-items: center;
  gap: 10px;
}
.right {
  display: flex;
  justify-content: center;
  align-items: start;
}

.upload-demo {
  display: block;

}
</style>