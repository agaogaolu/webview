<template>
  <el-container style="height: 100vh; width: 100vw;">
    <el-aside width="200px" style="background-color: #545c64;">
      <el-menu
        :default-active="activeMenu"
        class="el-menu-vertical-demo"
        background-color="#545c64"
        text-color="#fff"
        active-text-color="#ffd04b"
        style="display: flex; flex-direction: column; justify-content: center; align-items: center; height: 100%;"
      >
        <el-menu-item index="1" @click="selectMenu('upload')">
          <el-icon><upload /></el-icon>
          <span slot="title">上传图片功能</span>
        </el-menu-item>
        <el-menu-item index="2" @click="selectMenu('feature2')">
          <el-icon><view /></el-icon>
          <span slot="title">功能二</span>
        </el-menu-item>
        <el-menu-item index="3" @click="selectMenu('feature3')">
          <el-icon><setting /></el-icon>
          <span slot="title">功能三</span>
        </el-menu-item>
      </el-menu>
    </el-aside>

    <el-container class="right">
      <el-main>
        <component :is="currentComponent"></component>
      </el-main>
    </el-container>
  </el-container>
</template>

<script>
import UploadImage from './components/UploadImage.vue';
import FeatureTwo from './components/FeatureTwo.vue';
import FeatureThree from './components/FeatureThree.vue';
import { Upload, View, Setting } from '@element-plus/icons-vue';

export default {
  components: {
    UploadImage,
    FeatureTwo,
    FeatureThree,
    upload: Upload,
    view: View,
    setting: Setting,
  },
  data() {
    return {
      activeMenu: '1',
      currentFeature: 'upload',
    };
  },
  computed: {
    currentComponent() {
      switch (this.currentFeature) {
        case 'upload':
          return UploadImage;
        case 'feature2':
          return FeatureTwo;
        case 'feature3':
          return FeatureThree;
        default:
          return null;
      }
    },
  },
  methods: {
    selectMenu(feature) {
      this.currentFeature = feature;
      this.activeMenu = feature === 'upload' ? '1' : feature === 'feature2' ? '2' : '3';
    },
  },
};
</script>

<style scoped>
* {
  padding: 0;
  margin: 0;
}

.el-aside {
  display: flex;
  background-color: #545c64;
  color: #fff;
  padding: 0;
  align-items: center;
}

.el-menu-vertical-demo {
  border-right: none;
}

.el-menu-item {
  padding: 0 20px;
}

.el-menu-item .el-icon {
  margin-right: 10px;
}

.right {
  width: 100vh;
  display: flex;
  justify-content: center; /* 水平居中 */
  align-items: center; /* 垂直居中 */
}

.el-main {
  display: flex;
  justify-content: center; /* 水平居中 */
  align-items: center; /* 垂直居中 */
  width: 100vh;
}
</style>
