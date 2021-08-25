<template>
  <div>
    <div>
      <vue-draggable-resizable
        class="field"
        v-for="(rect, index) in boundedContext"
        :key="index"
        :x="rect.xUpperleft"
        :y="rect.yUpperleft"
        :w="rect.xLowerright"
        :h="rect.yLowerright"
        @activated="onActivated(index)"
        @dragstop="onDragStop"
        @resizestop="onResizeStop"
      >
        <div class="header">
          <h3>{{ rect.boundedContextName }}</h3>
        </div>
      </vue-draggable-resizable>
    </div>
    <div class="datalist1">
      <b-button class="addbtn" @click="addBoundedContext">+ 新增匡線</b-button>
      <div v-for="(p, pid) in boundedContext" :key="pid" style="padding: 10px;">
        <b-form-input
          v-model="p.boundedContextName"
          v-on:change="update(pid)"
        ></b-form-input>
        <b-button class="delbtn" @click="deleteBoundedContext(pid)"
          >刪除</b-button
        >
      </div>
    </div>
  </div>
</template>

<script>
import VueDraggableResizable from "vue-draggable-resizable";
import "vue-draggable-resizable/dist/VueDraggableResizable.css";
export default {
  name: "drag",
  components: {
    VueDraggableResizable,
  },
  data() {
    return {
      currentField: {},
      currentIndex: null,
      boundedContext: [
        // {
        //   description: "範例一",
        //   boundedContextName: "第一個匡線",
        //   xUpperleft: 400, // =x =left
        //   yUpperleft: 400, // =y =top
        //   xLowerright: 400, // =width
        //   yLowerright: 400, // = height
        // },
      ],
    };
  },
  mounted() {
    this.axios
      .get(
        "https://cip2021backend.cathaycloudteam.net/project/1/boundedContext"
      )
      .then((res) => {
        this.boundedContext = res.data;
      });
  },
  methods: {
    onActivated(index) {
      this.currentIndex = index;
      this.currentField.xUpperleft = this.boundedContext[
        this.currentIndex
      ].xUpperleft;
      this.currentField.yUpperleft = this.boundedContext[
        this.currentIndex
      ].yUpperleft;
      this.currentField.xLowerright = this.boundedContext[
        this.currentIndex
      ].xLowerright;
      this.currentField.yLowerright = this.boundedContext[
        this.currentIndex
      ].yLowerright;
    },
    onResizeStop(x, y, width, height) {
      console.log("x", x);
      console.log("y", y);
      console.log("width", width);
      console.log("height", height);
      this.boundedContext[this.currentIndex].xUpperleft = x;
      this.boundedContext[this.currentIndex].yUpperleft = y;
      this.boundedContext[this.currentIndex].xLowerright = width;
      this.boundedContext[this.currentIndex].yLowerright = height;
      this.currentField.xUpperleft = x;
      this.currentField.yUpperleft = y;
      this.currentField.xLowerright = width;
      this.currentField.yLowerright = height;
      try {
        if (this.currentIndex != -1) {
          this.update(this.currentIndex);
        }
      } catch (err) {
        alert(err);
      }
    },
    onDragStop(x, y) {
      console.log("x", x);
      console.log("y", y);
      this.boundedContext[this.currentIndex].xUpperleft = x;
      this.boundedContext[this.currentIndex].yUpperleft = y;
      this.currentField.xUpperleft = x;
      this.currentField.yUpperleft = y;
      try {
        if (this.currentIndex != -1) {
          this.update(this.currentIndex);
        }
      } catch (err) {
        alert(err);
      }
    },
    // below is old method

    async getBoundedContextId(pid) {
      try {
        let target = -1;
        await this.axios
          .get(
            "https://cip2021backend.cathaycloudteam.net/project/1/boundedContext"
          )
          .then((res) => {
            if (pid != -1) {
              target = res.data[pid].boundedContextId;
            } else {
              console.log("err");
            }
          });
        return target;
      } catch (err) {
        alert(err);
      }
    },
    async deleteBoundedContext(pid) {
      try {
        let target = await this.getBoundedContextId(pid);
        await this.axios
          .delete(
            `https://cip2021backend.cathaycloudteam.net/project/1/boundedContext/${target}`
          )
          .then(() => {
            this.boundedContext.splice(pid, 1);
          });
      } catch (err) {
        alert(err);
      }
    },
    async update(pid) {
      try {
        let target = await this.getBoundedContextId(pid);
        await this.axios.put(
          `https://cip2021backend.cathaycloudteam.net/project/1/boundedContext/${target}`,
          {
            description: this.boundedContext[pid].description, //name of the bounded context
            xUpperleft: this.boundedContext[pid].xUpperleft, // left
            yUpperleft: this.boundedContext[pid].yUpperleft, // top
            xLowerright: this.boundedContext[pid].xLowerright, // width
            yLowerright: this.boundedContext[pid].yLowerright, // height
            boundedContextName: this.boundedContext[pid].boundedContextName,
          }
        );
      } catch (err) {
        console.log(err);
      }
    },
    addBoundedContext() {
      this.axios
        .post(
          "https://cip2021backend.cathaycloudteam.net/project/1/boundedContext",
          {
            description: "文字一",
            // xUpperleft: this.left, // left
            // yUpperleft: this.top, // top
            // xLowerright: this.width, // width
            // yLowerright: this.height, // height
            xUpperleft: "250", // left
            yUpperleft: "300", // top
            xLowerright: "350", // width
            yLowerright: "400", // height
            boundedContextName: "文字二",
          }
        )
        .then((res) => {
          console.log(res.data);
          this.boundedContext.push(res.data);
        });
    },
  },
};
</script>

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
.field {
  position: absolute;
  border: 1px solid rgb(58, 58, 58);
  background-color: #d4d4d4;
  padding: 3px;
}
</style>
