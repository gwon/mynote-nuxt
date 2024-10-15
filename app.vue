<template>
  <div>
    <ResizableDraggable />
  </div>
</template>
<script>
import ResizableDraggable from "~/components/ResizableDraggable.vue";
export default {
  components: {
    ResizableDraggable,
  },
};
</script>
<style>
body {
  height: 100%;
  background: #9bff9b;
}
</style>
