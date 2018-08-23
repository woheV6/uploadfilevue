<template>
    <div>
             <div class="opration">
                <div>
                <div class="file">
                  <span>
                      <el-button size='mini'>选择文件</el-button>
                      <input type="file"
                         id="fileExcle"
                         ref="fileExcle"
                         class="upload-input-file"
                         v-on:change="handleFileUpload()"
                         name="upload"
                         :accept="accept" />
                  </span>
                </div>
                <div style="margin-top:20px">
                    <h3>文件名：{{fileExcleName}}</h3>
                    <h4>文件大小：{{fileSizeText}}</h4>
                </div>
                </div>
              </div>
              <span slot="footer" class="dialog-footer">
                <el-button @click="cleanFile">清除</el-button>
                <el-button type="primary" @click="submitFile" :diaabled="!fileExcle">确 定</el-button>
            </span>
    </div>
</template>
<script>
import axios from "axios";
export default {
  props: {
    up_load_url: {
      type: String,
      required: true
    },
    accept: {
      type: String
    },
    name: {
      default: _ => "file"
    },
    other_data: {}
  },
  data() {
    return {
      fileExcleName: "",
      fileSizeText: ""
    };
  },
  methods: {
    cleanFile() {
      this.fileExcle = null;
      this.fileSizeText = "";
      this.fileExcleName = "";
      this.$refs.fileExcle.value = "";
    },
    handleFileUpload() {
      if (!this.$refs.fileExcle.files[0]) return;
      this.fileExcle = this.$refs.fileExcle.files[0];
      console.log(this.$refs.fileExcle.files);
      const fileSzie = this.fileExcle.size / 1024 / 1024;
      if (fileSzie < 1) {
        this.fileSizeText = "小于1M";
      } else {
        this.fileSizeText = parseFloat(fileSzie).toFixed(2) + "M";
      }
      this.fileExcleName = this.fileExcle.name;
    },
    submitFile() {
      if (!this.fileExcle) {
        console.log("请先选择文件！");
        return;
      }
      const formData = new FormData();
      formData.append(this.name, this.fileExcle);
      if (this.other_data) {
        Object.keys(this.other_data).forEach(key => {
          formData.append(key, this.other_data[key]);
        });
      }
      const config = {
        headers: {
          "Content-Type": "multipart/form-data"
        }
      };
      axios.post(this.up_load_url, formData, config).then(res => {
        console.log("上传成功");
        // close_upload_plug 回调函数
        this.$emit("close_upload_plug", { flage: true, success: true });
      });
    }
  }
};
</script>
<style scoped>
.upload-input-file {
  position: absolute;
  opacity: 0;
  height: 30px;
  margin-left: -100px;
  cursor: pointer;
}
</style>
