<template>
  <div>
    <div id="drag" v-for="(p, pid) in boundingLine" :key="pid">
      <VueDragResize
        :isActive="true"
        :w="200"
        :h="200"
        v-on:resizing="resize"
        v-on:dragging="resize"
        style="background: #e6e6e6;"
      >
        <div class="header">
          <h3>{{ p.text }}</h3>
        </div>
      </VueDragResize>
    </div>
    <div class="datalist1">
      <b-button class="addbtn" @click="addBoundingLine">+ 新增匡線</b-button>
      <div v-for="(p, pid) in boundingLine" :key="pid" style="padding: 10px;">
        <b-form-input v-model="p.text"></b-form-input>
        <b-button class="delbtn" @click="boundingLine.splice(pid, 1)"
          >刪除</b-button
        >
      </div>
    </div>
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
      width: 0,
      height: 0,
      top: 0,
      left: 0,
      boundingLine: [
        // {
        //   text: "title",
        //   index: 0,
        // },
      ],
    };
  },

  methods: {
    resize(newRect) {
      this.width = newRect.width;
      this.height = newRect.height;
      this.top = newRect.top;
      this.left = newRect.left;
    },
    addBoundingLine() {
      this.boundingLine.push({
        text: "title",
        index: 0,
      });
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

.datalist1 {
  z-index: 999;
  padding-top: 20px;
  /* border: 10px solid yellow; */
  position: fixed;
  right: 225px;
  top: 40px;
  width: 185px;
  overflow-y: scroll;
  max-height: 96vh;
}
.addbtn {
  width: 120px;
  margin-left: 30px;
  z-index: 999;
}
.delbtn {
  margin-left: 50px;
  z-index: 999;
}
</style>
