<template>
  <quill-editor
  style="width: 220px;"
    class="editor"
    v-model="content"
    ref="myQuillEditor"
    :options="editorOption"
    @blur="onEditorBlur($event)"
    @focus="onEditorFocus($event)"
    @change="onEditorChange($event)"
  ></quill-editor>
</template>
<script>
import { quillEditor } from "vue-quill-editor";
export default {
  data() {
    return {
      content: null,
      editorOption: {
        modules: {
          toolbar: [
            [ "image"], // toggled buttons
            // ["blockquote", "code-block"]
          ]
        }
      }
    };
  },
  methods: {
    onEditorBlur(e, b, q) {},
    onEditorFocus(e, b, q) {
      //获得焦点事件
    },
    onEditorChange(e, b, q) {
        this.$emit('onChanges',e)
      //内容改变事件
    }
  }
};
</script>
 
<style>
</style>