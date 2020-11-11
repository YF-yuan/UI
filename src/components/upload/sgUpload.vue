<template>
  <!-- 上传单张 -->
  <div class="singleUpload">
    <a-upload
      action="https://www.mocky.io/v2/5cc8019d300000980a055e76"
      list-type="picture"
      :file-list="fileList"
      @change="handleChange"
    >
      <a-button> <a-icon type="upload" /> 上传单张图片 </a-button>
    </a-upload>

    <br />
  </div>
</template>

<script>
export default {
  name: 'SgUpload',
  data () {
    return {
      fileList: []
    }
  },
  methods: {
    handleChange (info) {
      let fileList = [...info.fileList]
      // 限制图片一次性展示的数量为4张
      fileList = fileList.slice(-4)
      fileList = fileList.map(file => {
        if (file.response) {
          file.url = file.response.url
        }
        return file
      })
      this.fileList = fileList
    }
  }
}
</script>

<style scoped>
/* tile uploaded pictures */
.upload-list-inline >>> .ant-upload-list-item {
  float: left;
  width: 200px;
  margin-right: 8px;
}
.upload-list-inline >>> .ant-upload-animate-enter {
  animation-name: uploadAnimateInlineIn;
}
.upload-list-inline >>> .ant-upload-animate-leave {
  animation-name: uploadAnimateInlineOut;
}
</style>
