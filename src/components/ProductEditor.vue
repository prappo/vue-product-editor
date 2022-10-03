<template>
  <div class="editor">
    <div class="topbar">Topbar</div>
    <div class="body">
      <div class="left-section">Left section</div>
      <div class="canvas-section" @mousewheel="onScroll">
        <div class="canvas-holder">
          <canvas
            :height="canvasHeight"
            :width="canvasWidth"
            class="canvas"
            ref="can"
          ></canvas>
        </div>
      </div>
      <div class="properties-section">Properties</div>
    </div>
  </div>
</template>

<script>
import { fabric } from "fabric";

export default {
  name: "ProductEditor",
  data() {
    return {
      c: null,
      canvasHeight: 570,
      canvasWidth: 570,
      imgHeight: null,
      imgWidth: null,
      img:
        "https://cdn.shopify.com/s/files/1/2303/2711/files/2_e822dae0-14df-4cb8-b145-ea4dc0966b34.jpg?v=1617059123",
    };
  },

  methods: {
    onScroll(event) {
      // if (scrollTop + clientHeight >= scrollHeight) {
      //   this.loadMorePosts();
      // }
      console.log(event.deltaY);
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

    canvas.on("mouse:wheel", function (opt) {
      var delta = opt.e.deltaY;
      var zoom = canvas.getZoom();
      zoom *= 0.999 ** delta;
      if (zoom > 20) zoom = 20;
      if (zoom < 0.01) zoom = 0.01;
      canvas.zoomToPoint({ x: opt.e.offsetX, y: opt.e.offsetY }, zoom);
      opt.e.preventDefault();
      opt.e.stopPropagation();
    });

    fabric.Image.fromURL(this.img, (img) => {
      img.set({
        left: 0,
        top: 0,
      });
      // console.log("Width " + img.width);
      // console.log("Height" + img.height);
      this.imgHeight = img.height;
      this.imgWidth = img.width;

      this.c.setWidth(this.imgWidth);
      this.c.setHeight(this.imgHeight);

      canvas.add(img);
    });

    // canvas.setDimensions(
    //   { width: this.imgWidth, height: this.imgHeight },
    //   { backstoreOnly: true }
    // );

    // console.log("canvas height " + canvas.height);
    // console.log("new" + ref.height);
    // console.log("new" + ref.width);
    // canvas.height = this.canvasHeight;
    // canvas.width = this.canvasWidth;
    // canvas.requestRenderAll();
  },
};
</script>
<style scoped>
.editor {
  background: #1A2730;
  padding: 15px;
  margin: 0px;
  color: #F7F4F3;
  height: 100vh;
}
.body {
  display: flex;
  justify-content: space-between;
}

.canvas-section {
  background: #071822;
  height: 100vh;
  width: 60%;
  display: flex;
  justify-content: center;
  align-items: center;
}
.topbar {
  border-bottom: 1px solid #B5B9BD;
}
.properties-section {
  width: 20%;
}
.left-section {
  width: 20%;
}
.canvas {
  opacity: 1;
  display: block;
  width: 100%;
  transition: 0.5s ease;
  backface-visibility: hidden;
  border-radius: 10px;
  background-image: url("data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAAwAAAAMCAIAAADZF8uwAAAACXBIWXMAAAsTAAALEwEAmpwYAAAFyGlUWHRYTUw6Y29tLmFkb2JlLnhtcAAAAAAAPD94cGFja2V0IGJlZ2luPSLvu78iIGlkPSJXNU0wTXBDZWhpSHpyZVN6TlRjemtjOWQiPz4gPHg6eG1wbWV0YSB4bWxuczp4PSJhZG9iZTpuczptZXRhLyIgeDp4bXB0az0iQWRvYmUgWE1QIENvcmUgNS42LWMxNDAgNzkuMTYwNDUxLCAyMDE3LzA1LzA2LTAxOjA4OjIxICAgICAgICAiPiA8cmRmOlJERiB4bWxuczpyZGY9Imh0dHA6Ly93d3cudzMub3JnLzE5OTkvMDIvMjItcmRmLXN5bnRheC1ucyMiPiA8cmRmOkRlc2NyaXB0aW9uIHJkZjphYm91dD0iIiB4bWxuczp4bXA9Imh0dHA6Ly9ucy5hZG9iZS5jb20veGFwLzEuMC8iIHhtbG5zOnhtcE1NPSJodHRwOi8vbnMuYWRvYmUuY29tL3hhcC8xLjAvbW0vIiB4bWxuczpzdEV2dD0iaHR0cDovL25zLmFkb2JlLmNvbS94YXAvMS4wL3NUeXBlL1Jlc291cmNlRXZlbnQjIiB4bWxuczpkYz0iaHR0cDovL3B1cmwub3JnL2RjL2VsZW1lbnRzLzEuMS8iIHhtbG5zOnBob3Rvc2hvcD0iaHR0cDovL25zLmFkb2JlLmNvbS9waG90b3Nob3AvMS4wLyIgeG1wOkNyZWF0b3JUb29sPSJBZG9iZSBQaG90b3Nob3AgQ0MgKE1hY2ludG9zaCkiIHhtcDpDcmVhdGVEYXRlPSIyMDE4LTAzLTEwVDIyOjE4OjE2KzAyOjAwIiB4bXA6TWV0YWRhdGFEYXRlPSIyMDE4LTAzLTEwVDIyOjE4OjE2KzAyOjAwIiB4bXA6TW9kaWZ5RGF0ZT0iMjAxOC0wMy0xMFQyMjoxODoxNiswMjowMCIgeG1wTU06SW5zdGFuY2VJRD0ieG1wLmlpZDozZDQ2Y2ZkMS1hNDk4LTQxMTEtYTBhNS1jNDEyMjRiMWNkYTIiIHhtcE1NOkRvY3VtZW50SUQ9ImFkb2JlOmRvY2lkOnBob3Rvc2hvcDphNzI4YWUwNi1mNmRmLTJiNDUtYmVjOS0yNGRhMmZlODQzOGUiIHhtcE1NOk9yaWdpbmFsRG9jdW1lbnRJRD0ieG1wLmRpZDpmM2MxMTQ3MS1lNzk5LTQ4MjQtYmZhMS1mYzgwMmI0MzJhMmYiIGRjOmZvcm1hdD0iaW1hZ2UvcG5nIiBwaG90b3Nob3A6Q29sb3JNb2RlPSIzIj4gPHhtcE1NOkhpc3Rvcnk+IDxyZGY6U2VxPiA8cmRmOmxpIHN0RXZ0OmFjdGlvbj0iY3JlYXRlZCIgc3RFdnQ6aW5zdGFuY2VJRD0ieG1wLmlpZDpmM2MxMTQ3MS1lNzk5LTQ4MjQtYmZhMS1mYzgwMmI0MzJhMmYiIHN0RXZ0OndoZW49IjIwMTgtMDMtMTBUMjI6MTg6MTYrMDI6MDAiIHN0RXZ0OnNvZnR3YXJlQWdlbnQ9IkFkb2JlIFBob3Rvc2hvcCBDQyAoTWFjaW50b3NoKSIvPiA8cmRmOmxpIHN0RXZ0OmFjdGlvbj0ic2F2ZWQiIHN0RXZ0Omluc3RhbmNlSUQ9InhtcC5paWQ6M2Q0NmNmZDEtYTQ5OC00MTExLWEwYTUtYzQxMjI0YjFjZGEyIiBzdEV2dDp3aGVuPSIyMDE4LTAzLTEwVDIyOjE4OjE2KzAyOjAwIiBzdEV2dDpzb2Z0d2FyZUFnZW50PSJBZG9iZSBQaG90b3Nob3AgQ0MgKE1hY2ludG9zaCkiIHN0RXZ0OmNoYW5nZWQ9Ii8iLz4gPC9yZGY6U2VxPiA8L3htcE1NOkhpc3Rvcnk+IDwvcmRmOkRlc2NyaXB0aW9uPiA8L3JkZjpSREY+IDwveDp4bXBtZXRhPiA8P3hwYWNrZXQgZW5kPSJyIj8+xCjdVgAAAG9JREFUGJV9kEEKAkEMBKsG74L4/xeKqCAe20OYXWVmpy+5dCrp9vX+AIAAhjzvN/7VBMuAEJmoBUJESDbiSCplxthJQlYemp3RkRP36TFkOV+uYn5CtHHPHjML0/afWuePSABJlbIiKXr80z6qoC+9xiTGq4ToLAAAAABJRU5ErkJggg==");
  align-self: center;
  /* max-width: 100%;
  max-height: 100%; */
}
.canvas-holder {
  transform: scale(0.5);
}
</style>