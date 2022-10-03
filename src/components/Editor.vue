<template>
  <div class="editor-body">
    <div>
      <canvas ref="can" :width="canvasWidth" :height="canvasHeight"></canvas>
    </div>
    <div>
      Selected {{ colorPickerSelected }}
      <select v-model="colorPickerSelected">
        <option
          v-for="(option, index) in colorPickerOptions"
          :key="index"
          :value="option.value"
        >
          {{ option.text }}
        </option>
      </select>
      <material-picker
        v-if="colorPickerSelected == 'material-picker'"
        v-model="colors"
      />
      <compact-picker
        v-if="colorPickerSelected == 'compact-picker'"
        v-model="colors"
      />
      <swatches-picker
        v-if="colorPickerSelected == 'swatches-picker'"
        v-model="colors"
      />
      <slider-picker
        v-if="colorPickerSelected == 'slider-picker'"
        v-model="colors"
      />
      <sketch-picker
        v-if="colorPickerSelected == 'sketch-picker'"
        v-model="colors"
      />
      <chrome-picker
        v-if="colorPickerSelected == 'chrome-picker'"
        v-model="colors"
      />
    </div>
  </div>
</template>

<script>
import { fabric } from "fabric";
import { Material, Compact, Swatches, Slider, Sketch, Chrome } from "vue-color";

export default {
  name: "EditorModal",
  components: {
    "material-picker": Material,
    "compact-picker": Compact,
    "swatches-picker": Swatches,
    "slider-picker": Slider,
    "sketch-picker": Sketch,
    "chrome-picker": Chrome,
  },
  props: ["title", "img"],
  data() {
    return {
      c: null,
      colors: "#194d33",
      colorPickerSelected: "slider-picker",
      colorPickerOptions: [
        { value: "material-picker", text: "Material" },
        { value: "compact-picker", text: "Compact" },
        { value: "swatches-picker", text: "Swatches" },
        { value: "slider-picker", text: "Slider" },
        { value: "sketch-picker", text: "Sketch" },
        { value: "chrome-picker", text: "Chrome" },
      ],
      canvasWidth: 680,
      canvasHeight: 500,
    };
  },
  methods: {
    closeModal() {
      console.log("fired");
      this.$emit("ce", 1);
    },
    doSomething() {
      console.log(this.c);
      this.c.backgroundColor = "blue";
      this.c.renderAll();
    },
  },
  watch: {
    colors: function (oldValue, newValue) {
      this.c.backgroundColor = oldValue.hex;
      this.c.renderAll();
      console.log(newValue.hex);
    },
  },
  mounted() {
    const ref = this.$refs.can;
    const canvas = new fabric.Canvas(ref);
    const rect = new fabric.Rect({
      fill: "red",
      width: 20,
      height: 20,
    });

    this.c = canvas;
    canvas.backgroundColor = this.colors;
    canvas.add(rect);

    fabric.Image.fromURL(this.img, (img) => {
      img.set({
        left: 0,
        top: 0,
      });
      img.scaleToHeight(300);
      img.scaleToWidth(300);
      canvas.add(img);
    });

    fabric.Image.fromURL(this.img, (img) => {
      img.set({
        left: 300,
        top: 0,
      });
      img.scaleToHeight(300);
      img.scaleToWidth(300);
      canvas.add(img);
    });
  },
};
</script>

<style scoped>
.editor-body {
  display: flex;
  justify-content: space-between;
}
</style>