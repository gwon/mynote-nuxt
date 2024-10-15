<template>
  <div
    ref="container"
    class="resizable-draggable"
    :style="{
      width: width + 'px',
      height: height + 'px',
      left: x + 'px',
      top: y + 'px',
    }"
    @mousedown="startDrag"
  >
    {{ content }}
    <div class="resize-handle" @mousedown.stop="startResize"></div>
  </div>
</template>

<script>
export default {
  props: {
    content: {
      type: String,
      default: "Resizable and Draggable",
    },
    initialWidth: {
      type: Number,
      default: 200,
    },
    initialHeight: {
      type: Number,
      default: 100,
    },
    initialX: {
      type: Number,
      default: 0,
    },
    initialY: {
      type: Number,
      default: 0,
    },
  },
  data() {
    return {
      width: this.initialWidth,
      height: this.initialHeight,
      x: this.initialX,
      y: this.initialY,
      dragging: false,
      resizing: false,
      dragStartX: 0,
      dragStartY: 0,
      resizeStartX: 0,
      resizeStartY: 0,
      resizeStartWidth: 0,
      resizeStartHeight: 0,
    };
  },
  mounted() {
    document.addEventListener("mousemove", this.onMouseMove);
    document.addEventListener("mouseup", this.onMouseUp);
  },
  beforeDestroy() {
    document.removeEventListener("mousemove", this.onMouseMove);
    document.removeEventListener("mouseup", this.onMouseUp);
  },
  methods: {
    startDrag(e) {
      this.dragging = true;
      const rect = this.$refs.container.getBoundingClientRect();
      this.dragStartX = e.clientX - rect.left;
      this.dragStartY = e.clientY - rect.top;
    },
    startResize(e) {
      e.stopPropagation();
      this.resizing = true;
      this.resizeStartX = e.clientX;
      this.resizeStartY = e.clientY;
      this.resizeStartWidth = this.width;
      this.resizeStartHeight = this.height;
    },
    onMouseMove(e) {
      if (this.dragging) {
        this.x = e.clientX - this.dragStartX;
        this.y = e.clientY - this.dragStartY;
      } else if (this.resizing) {
        const dx = e.clientX - this.resizeStartX;
        const dy = e.clientY - this.resizeStartY;
        this.width = Math.max(50, this.resizeStartWidth + dx);
        this.height = Math.max(50, this.resizeStartHeight + dy);
      }
    },
    onMouseUp() {
      this.dragging = false;
      this.resizing = false;
    },
  },
};
</script>

<style scoped>
.resizable-draggable {
  position: absolute;
  background-color: #f0f0f0;
  border: 1px solid #ccc;
  cursor: move;
  user-select: none;
}

.resize-handle {
  position: absolute;
  right: 0;
  bottom: 0;
  width: 10px;
  height: 10px;
  background-color: #ccc;
  cursor: se-resize;
}
</style>
