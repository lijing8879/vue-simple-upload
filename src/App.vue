<template>
  <div id="app">
    <h3>Vue Simple Upload</h3>
    <vue-simple-upload
      :options="options"
      @progress-update="progressUpdate"
      @file-size-error="fileSizeError"
      ref="fileUploadComp"
    >
    </vue-simple-upload>
    <br>
    <div class="upload-file-info-section" v-show="fileInfoList.length > 0">
      <h4 class="section-title">Files selected</h4>
      <div class="file-info-item" v-for="(fileInfo, index) in fileInfoList" :key="index">
        <div class="progress-info">
          <span class="file-name">{{ fileInfo.fileName }}</span>
          <span class="file-size">{{ parseInt(fileInfo.fileInfo.size / 1000, 10) }}kb</span>
          <span class="file-progress">{{ fileInfo.progress }}</span>
          <span class="operate">
            <i class="fa fa-arrow-up fl-l" v-if="fileInfo.type === 'waiting'" @click="startUpload(fileInfo.id)"></i>
            <i class="fa fa-check fl-r" v-if="fileInfo.type === 'success'"></i>
            <i class="fa fa-times fl-r" v-if="fileInfo.type === 'fail' || fileInfo.type === 'abort'"></i>
            <i class="fa fa-trash fl-r" v-if="fileInfo.type === 'uploading'" @click="abortUpload(fileInfo.id)"></i>
          </span>
        </div>
        <div
          class="progress-bg"
          :class="fileInfo.type"
          :style="{ width: fileInfo.type === 'uploading' ? fileInfo.progress : '100%' }"
        ></div>
      </div>
    </div>
  </div>
</template>

<script>
import { VueSimpleUpload } from './lib/index.js'

export default {
  components: { VueSimpleUpload },

  data() {
    return {
      options: {
        className: 'btn-solid',
        btnContent: 'Choose File',
        url: '/api/files/upload',
        accept: '*',
        multiple: true,
        // autoStart: false,
        size: 100000
      },
      imageUrl: '',
      fileInfoList: []
    }
  },

  methods: {
    fileSizeError(fileNames) {
      console.error('file blow are oversized: ', ...fileNames)
    },

    progressUpdate(fileInfoList) {
      this.fileInfoList = fileInfoList
    },

    abortUpload(id) {
      this.$refs.fileUploadComp.abort(id)
    },

    startUpload(id) {
      this.$refs.fileUploadComp.startUpload(id)
    }
  }
}
</script>

<style>
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}

.image-show-section {
  max-width: 650px;
  max-height: 400px;
  border-radius: 5px;
}

h1,
h2 {
  font-weight: normal;
}

ul {
  list-style-type: none;
  padding: 0;
}

li {
  display: inline-block;
  margin: 0 10px;
}

a {
  color: #42b983;
}

.fl-l {
  float: left;
}

.fl-r {
  float: right;
}

.upload-file-info-section {
  width: 800px;
  display: inline-block;
  margin-top: 40px;
  border: 1px solid #ddd;
}

.section-title {
  margin: 0;
  font-weight: normal;
  text-align: left;
  line-height: 40px;
  text-indent: 20px;
  border-bottom: 1px solid #ddd;
}

.file-info-item {
  position: relative;
  height: 45px;
  line-height: 45px;
  background-color: #f5f7fa;
  text-align: left;
}

.file-info-item::not(:first-child) {
  border-top: 1px solid #dcdfe5;
}

.progress-info {
  position: absolute;
  top: 0;
  height: 100%;
}

.progress-bg {
  height: 100%;
}

.progress-bg.uploading {
  background-color: #e5f2ff;
}

.progress-bg.success {
  background-color: #e5ffef;
}

.progress-bg.abort {
  background-color: #fff6e5;
}

.progress-bg.error {
  background-color: #ffe5ea;
}

.file-name {
  display: inline-block;
  width: 480px;
  padding-left: 20px;
}

.file-size,
.file-progress,
.operate {
  display: inline-block;
  width: 100px;
  vertical-align: top;
}

.operate {
  width: 70px;
  height: 45px;
}

.fa {
  width: 20px;
  height: 20px;
  line-height: 20px;
  margin-top: 12px;
  text-align: center;
}
</style>
