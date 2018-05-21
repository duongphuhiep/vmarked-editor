<template>
  <div ref="editor">
    <slot></slot>
  </div>
</template>
<script>
import ace from "ace-builds";
import textmate from "ace-builds/src-noconflict/theme-textmate";
import "ace-builds/src-noconflict/mode-markdown";

export default {
  name: "MdEditor",
  model: {
    event: "change"
  },
  data() {
    return {
      editor: null
    };
  },
  mounted() {
    this.editor = ace.edit(this.$refs.editor, {
      showPrintMargin: false,
      wrap: true,
      foldStyle: "markbeginend",
      theme: textmate,
      mode: "ace/mode/markdown"
    });
    this.editor.session.on("change", () => {
      let cvalue = this.editor.getValue();
      //this.content = cvalue;
      this.$emit("change", cvalue);
    });
    this.$emit("change", this.editor.getValue());
  }
};
</script>
