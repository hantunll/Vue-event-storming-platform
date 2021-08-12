<template>
  <div id="drag">
    <VueDragResize
      :isActive="true"
      :w="200"
      :h="200"
      v-on:resizing="resize"
      v-on:dragging="resize"
      style="background: #e6e6e6;"
    >
      <div class="header">
        <h3>{{ title }}</h3>
      </div>
    </VueDragResize>
  </div>
</template>

<script>
import VueDragResize from "vue-drag-resize";
export default {
  name: "drag",
  // props: ["title"],
  components: {
    VueDragResize,
  },

  data() {
    return {
      title: "title",
      width: 0,
      height: 0,
      top: 0,
      left: 0,
    };
  },

  methods: {
    resize(newRect) {
      this.width = newRect.width;
      this.height = newRect.height;
      this.top = newRect.top;
      this.left = newRect.left;
    },
  },
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
.header {
  /* padding: 1px; */
  margin-top: 5px;
  background-color: #b3b3b3;
  color: rgb(235, 232, 229);
  cursor: grab;
  text-align: center;
}
</style>
