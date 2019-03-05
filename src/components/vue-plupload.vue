<template>
    <input ref="btn" :class="className" :value="text" type="button" />
</template>
<style>
.excel2json {
    color: #ffffff;
    background-color: #337ab7;
    display: inline-block;
    padding: 0.5em 1em;
    margin: 0;
    text-align: center;
    vertical-align: middle;
    touch-action: manipulation;
    cursor: pointer;
    user-select: none;
    border: 0 solid transparent;
    border-radius: 4px;
}
</style>
<script>
import Vue from 'vue';
const plupload = require("plupload");

const uploaderDefaultOption = {
    runtimes: 'html5,html4'
};

export default{

    props: {
        className: {
            type: String,
            default: "plupload-btn"
        },
        text: {
            type: String,
            default: "Upload"
        },
        options: {
            type: Object,
            default: function () {
                return {

                }
            }
        }
    },
    
    methods: {
        renderUI: function() {
            let self = this;

            Vue.nextTick(() => {

                let opt = Object.assign(uploaderDefaultOption, self.options, {

                    browse_button: self.$refs.btn,

                    init: {
                        PostInit: function (uploader) {
                            self.$emit("init", uploader)
                        },

                        FilesAdded: function (up, files) {
                            self.$emit("added", up, files)
                        },

                        UploadProgress: function (up, file) {
                            self.$emit("progress", up, file)
                        },

                        FileUploaded: function(up, file, result) {
                            self.$emit("uploaded", up, file, result)
                        },

                        Error: function (up, err) {
                            self.$emit("error", up, err)
                        }
                    }
                });

                let uploader = new plupload.Uploader(opt);

                uploader.init();
            });
        }
    },

    mounted: function () {
        this.renderUI();
    },
    
    updated: function() {
        this.renderUI();
    }
};

</script>
