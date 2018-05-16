<template>
  <div>
    <textarea class="half" v-model="md"></textarea>
		<div class="half preview" v-html="highlightedHtml" ref="preview"></div>
  </div>
</template>

<script>
import marked from "marked";
import _ from "lodash";
import diffDOM from "diff-dom";
import { JSDOM } from "jsdom";

export default {
  name: "VMarkedEditor",
  data() {
    return {
      md: "",
      oldHtml: "",
      html: "",
      highlightedHtml: ""
    };
  },
  props: {
    content: String
  },
  mounted() {
    this.md = this.content;
  },
  methods: {
    render: _.debounce(function() {
      this.oldHtml = this.html;
      this.html = marked(this.md, { headerIds: false });
      this.highlightedHtml = highlightDiff(this.oldHtml, this.html);
    }, 300),
    updateScroll: _.debounce(function() {
      let p = this.$refs.preview;
      let h = p.getElementsByClassName("highlight")[0];
      if (h) {
        //console.log("offsetTop =", h.offsetTop);
        p.scrollTop = h.offsetTop - 200; //height/2
      }
    }, 300)
  },
  watch: {
    md: "render",
    highlightedHtml: "updateScroll"
  }
};

function highlightDiff(html1, html2) {
  const dom1 = new JSDOM(html1);
  const dom2 = new JSDOM(html2);
  var diffDomPatch = new diffDOM().diff(
    dom1.window.document,
    dom2.window.document
  );
  //console.log(diffDomPatch);
  diffDomPatch.forEach(d => {
    if (d.action.startsWith("add") || d.action.startsWith("modify")) {
      let e = dom2.window.document;
      d.route.forEach(i => {
        e = e.childNodes[i];
      });
      if (e.nodeType == 3) {
        //textNode, highlight parent
        e = e.parentNode;
      }
      //console.log("Node to highlight", e);
      e.className += " highlight";
    }
  });

  return dom2.serialize();
}
</script>

<style scoped>
* {
  box-sizing: border-box;
}
textarea {
  width: 100%;
  height: 100%;
  resize: vertical;
}
.half {
  float: left;
  width: 50%;
  height: 400px;
}
.half >>> .highlight {
  animation: highlightAnim 5s;
}
.preview {
  padding-left: 5px;
  overflow: scroll;
  resize: vertical;
  max-height: 400px;
}
@media only screen and (max-width: 768px) {
  .half {
    float: left;
    width: 100%;
    height: 400px;
  }
}
@keyframes highlightAnim {
  0% {
    background: lightblue;
  }
  100% {
    background: none;
  }
}
</style>
