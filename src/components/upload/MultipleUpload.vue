<template>
  <!-- 上传多张图片 照片墙 -->
  <div class="clearfix">
    <a-upload
      action="https://www.mocky.io/v2/5cc8019d300000980a055e76"
      list-type="picture-card"
      :file-list="fileList"
      :multiple="true"
      @preview="handlePreview"
      @change="handleChange"
      :beforeUpload="beforeUpload"
    >
      <!-- 图片数量小于9时显示添加 -->
      <div v-if="fileList.length < 9">
        <a-icon type="plus" />
        <div class="ant-upload-text">Upload</div>
      </div>
    </a-upload>
    <a-modal :visible="previewVisible" :footer="null" @cancel="handleCancel">
      <img alt="example" style="width: 100%" :src="previewImage" />
    </a-modal>
  </div>
</template>
<script>
function getBase64 (file) {
  return new Promise((resolve, reject) => {
    const reader = new FileReader()
    reader.readAsDataURL(file)
    reader.onload = () => resolve(reader.result)
    reader.onerror = (error) => reject(error)
  })
}
export default {
  data () {
    return {
      previewVisible: false,
      previewImage: '',
      fileList: []
    }
  },
  methods: {
    handleCancel () {
      this.previewVisible = false
    },
    async handlePreview (file) {
      if (!file.url && !file.preview) {
        file.preview = await getBase64(file.originFileObj)
      }
      this.previewImage = file.url || file.preview
      this.previewVisible = true
    },
    handleChange ({ fileList }) {
      console.log(fileList)
      for (let i = 0; i < fileList.length; i++) {
        if (fileList[i].type === '') {
          fileList = fileList.splice(i, 1)
        }
      }
      this.fileList = fileList // filelist是已经上传的全部img列表 file是正在上传的单个img
    },
    // 限制图片的格式、大小、分辨率等
    beforeUpload (file) {
      const imgTypes = file.type === 'image/jpeg' || file.type === 'image/png' || file.type === 'image/jpg'
      if (!imgTypes) {
        this.$message.error('仅支持jpg/jpeg/png格式!')
      }
      const imgSize = file.size / 1024 / 1024 < 5
      if (!imgSize) {
        this.$message.error('上传图片大小不能超过5MB!')
      }
      return imgTypes && imgSize
    }
  }
}
</script>
<style>
/* you can make up upload button and sample style by using stylesheets */
.ant-upload-select-picture-card i {
  font-size: 32px;
  color: #999;
}

.ant-upload-select-picture-card .ant-upload-text {
  margin-top: 8px;
  color: #666;
}
</style>
