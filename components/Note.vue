<template>
  <section class="note">
    <input
      v-model="note.title"
      type="text"
      class="note-title"
      placeholder="Untitled"
    />
    <client-only>
      <quill-editor
        ref="myQuillEditor"
        v-model="note.content"
        :options="editorOption"
        @keyup="$emit('update:note', note)"
        @blur="onEditorBlur($event)"
        @focus="onEditorFocus($event)"
        @ready="onEditorReady($event)"
      />
    </client-only>
  </section>
</template>

<script>
export default {
  props: {
    note: Object
  },
  data() {
    return {
      editorOption: {
        theme: "snow",
        modules: {
          toolbar: [
            ["bold", "italic", "underline", "strike"], // toggled buttons
            ["blockquote", "code-block"],

            [{ header: 1 }, { header: 2 }], // custom button values
            [{ list: "ordered" }, { list: "bullet" }],
            [{ script: "sub" }, { script: "super" }], // superscript/subscript
            [{ indent: "-1" }, { indent: "+1" }], // outdent/indent
            [{ direction: "rtl" }], // text direction

            [{ size: ["small", false, "large", "huge"] }], // custom dropdown
            [{ header: [1, 2, 3, 4, 5, 6, false] }],

            [{ color: [] }, { background: [] }], // dropdown with defaults from theme
            [{ font: [] }],
            [{ align: [] }],

            ["clean"]
          ]
        }
      }
    };
  },
  mounted() {
    console.log("App init, this quill insrance object is:", this.myQuillEditor);
    setTimeout(() => {
      this.content = "I am changed";
    }, 3000);
  },
  methods: {
    onEditorBlur(editor) {},
    onEditorFocus(editor) {},
    onEditorReady(editor) {}
  }
};
</script>

<style></style>
