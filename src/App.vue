<template>
  <div>
    <product-editor></product-editor>
  </div>
</template>

<script>
import Editor from "./components/Editor.vue";
import ProductEditor from "./components/ProductEditor.vue";

export default {
  name: "Myapp",
  components: { ProductEditor },
  methods: {
    openModal() {
      this.$modal.show(
        Editor,
        {
          title: "The standard Lorem Ipsum passage",
          img:
            "https://99designs-blog.imgix.net/blog/wp-content/uploads/2018/06/attachment_77536106-e1528674476680.jpg?auto=format&q=60&w=1480&h=1480&fit=crop&crop=faces",
        },
        {
          width: "90%",
          height: "90%",
          adaptive: true,
        }
      );
    },
    closeModal() {
      console.log("closed");
      this.$modal.hide(Editor);
    },
  },
  created() {
    this.$on("cm", (id) => {
      console.log("Received " + id);
      this.closeModal();
    });
  },
};
</script>
