<template>
  <div id="app">
    <div class="excel2json-wrap">
      <vue-pluload
        v-if="!loadingFlag"
        @added="handleAdded"
        :options="UploadOpt"
        text="Select a xlsx file"
        :class-name="className"
      ></vue-pluload>
      <button :class="className" v-if="loadingFlag">Loading</button>
      <span v-if="tips" class="error">{{tips}}</span>
    </div>
  </div>
</template>
<style lang="scss">
.excel2json-wrap {
  display: inline-block;
}
.error {
  color: red;
}
</style>
<script>
import VuePlupload from "./vue-plupload.vue";
import XLSX from "xlsx";
export default {
  props: {
    className: {
      type: String,
      default: "excel2json"
    }
  },

  data() {
    return {
      tips: false,
      loadingFlag: false,
      OUTPUT: {},
      UploadOpt: {
        filters: [{ title: "Excel files", extensions: "xlsx,xls" }],
        multi_selection: false
      }
    };
  },

  methods: {
    handleAdded(uploader, files) {
      let file = files.length > 0 ? files[0] : null;
      this.loadingFlag = true;
      if (file) {
        this.OUTPUT = {};
        this.tips = false;
        let reader = new FileReader();
        reader.onload = e => {
          try {
            let workbook = XLSX.read(e.target.result, {
              type: "binary"
            });
            workbook.SheetNames.forEach(sheet => {
              this.$set(
                this.OUTPUT,
                sheet,
                XLSX.utils.sheet_to_json(workbook.Sheets[sheet], { header: 1 })
              );
            });
            this.$emit("parsed", this.OUTPUT);
          } catch (error) {
            this.tips = error;
            this.$emit("error", error);
          }
          this.loadingFlag = false;
        };
        try {
          reader.readAsBinaryString(file.getNative());
        } catch (error) {
          this.tips = error;
          this.loadingFlag = false;
          this.$emit("error", error);
        }
      }
    }
  },

  components: {
    "vue-pluload": VuePlupload
  }
};
</script>
