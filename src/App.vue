<template>
  <div id="app">
    <excel-json @parsed="handleParsed" @error="handleError"></excel-json>
    <pre>{{PreObj}}</pre>
  </div>
</template>
<script>
import Excel2JSON from "./components/excel2json.vue";

export default {
  data() {
    return {
      OUTPUT: {}
    };
  },

  computed: {
    PreObj: vm => {
      return JSON.stringify(vm.OUTPUT, 0, 4);
    }
  },

  methods: {
    handleParsed(args) {
      this.OUTPUT = args;
    },
    handleError(args) {
      console.log(args);
    }
  },

  components: {
    "excel-json": Excel2JSON
  }
};
</script>