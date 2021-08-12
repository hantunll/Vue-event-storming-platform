<template>
  <div id="app">
    <transition-group tag="ul" name="fade">
      <div
        class="postit text"
        v-for="(p, pid) in postits"
        :key="pid"
        :style="postitCss(p)"
        @mousedown="onmousedown($event, pid)"
        @mousemove="onmousemove($event)"
        @mouseup="onmouseup()"
      >
        {{ p.text }}
        <div class="colorList">
          <div
            class="block"
            v-for="(color, id) in colorList"
            :key="id"
            :style="{ backgroundColor: color.color }"
            @click="p.color = color.name"
          ></div>
        </div>
        <div class="buttonList">
          <b-button @click="setText(pid)">
            <svg
              xmlns="http://www.w3.org/2000/svg"
              width="16"
              height="16"
              fill="currentColor"
              class="bi bi-pencil-square"
              viewBox="0 0 16 16"
            >
              <path
                d="M15.502 1.94a.5.5 0 0 1 0 .706L14.459 3.69l-2-2L13.502.646a.5.5 0 0 1 .707 0l1.293 1.293zm-1.75 2.456-2-2L4.939 9.21a.5.5 0 0 0-.121.196l-.805 2.414a.25.25 0 0 0 .316.316l2.414-.805a.5.5 0 0 0 .196-.12l6.813-6.814z"
              />
              <path
                fill-rule="evenodd"
                d="M1 13.5A1.5 1.5 0 0 0 2.5 15h11a1.5 1.5 0 0 0 1.5-1.5v-6a.5.5 0 0 0-1 0v6a.5.5 0 0 1-.5.5h-11a.5.5 0 0 1-.5-.5v-11a.5.5 0 0 1 .5-.5H9a.5.5 0 0 0 0-1H2.5A1.5 1.5 0 0 0 1 2.5v11z"
              />
            </svg>
          </b-button>
          <b-button class="btnDown" @click="postits.splice(pid, 1)">
            <svg
              xmlns="http://www.w3.org/2000/svg"
              width="16"
              height="16"
              fill="currentColor"
              class="bi bi-trash"
              viewBox="0 0 16 16"
            >
              <path
                d="M5.5 5.5A.5.5 0 0 1 6 6v6a.5.5 0 0 1-1 0V6a.5.5 0 0 1 .5-.5zm2.5 0a.5.5 0 0 1 .5.5v6a.5.5 0 0 1-1 0V6a.5.5 0 0 1 .5-.5zm3 .5a.5.5 0 0 0-1 0v6a.5.5 0 0 0 1 0V6z"
              />
              <path
                fill-rule="evenodd"
                d="M14.5 3a1 1 0 0 1-1 1H13v9a2 2 0 0 1-2 2H5a2 2 0 0 1-2-2V4h-.5a1 1 0 0 1-1-1V2a1 1 0 0 1 1-1H6a1 1 0 0 1 1-1h2a1 1 0 0 1 1 1h3.5a1 1 0 0 1 1 1v1zM4.118 4 4 4.059V13a1 1 0 0 0 1 1h6a1 1 0 0 0 1-1V4.059L11.882 4H4.118zM2.5 3V2h11v1h-11z"
              />
            </svg>
          </b-button>
        </div>
      </div>
      <!-- 便利貼下方的工具欄 -->
    </transition-group>
    <div class="datalist">
      <b-button @click="addPostit" style="margin-left: 30px; margin-top: 20px;">
        + 新增便利貼
      </b-button>
      <div v-for="(p, pid) in postits" :key="pid" style="padding: 10px">
        <!-- <input v-model="p.text" /> -->
        <b-form-input v-model="p.text"></b-form-input>
        <!-- 不能叫colorList不然會被跟者改 -->
        <div class="controlColor">
          <div
            class="block"
            v-for="(color, id) in colorList"
            :key="id"
            :style="{ backgroundColor: color.color }"
            @click="p.color = color.name"
          ></div>
          <b-button class="deleteButton" @click="postits.splice(pid, 1)"
            ><svg
              xmlns="http://www.w3.org/2000/svg"
              width="16"
              height="16"
              fill="currentColor"
              class="bi bi-trash"
              viewBox="0 0 16 16"
            >
              <path
                d="M5.5 5.5A.5.5 0 0 1 6 6v6a.5.5 0 0 1-1 0V6a.5.5 0 0 1 .5-.5zm2.5 0a.5.5 0 0 1 .5.5v6a.5.5 0 0 1-1 0V6a.5.5 0 0 1 .5-.5zm3 .5a.5.5 0 0 0-1 0v6a.5.5 0 0 0 1 0V6z"
              />
              <path
                fill-rule="evenodd"
                d="M14.5 3a1 1 0 0 1-1 1H13v9a2 2 0 0 1-2 2H5a2 2 0 0 1-2-2V4h-.5a1 1 0 0 1-1-1V2a1 1 0 0 1 1-1H6a1 1 0 0 1 1-1h2a1 1 0 0 1 1 1h3.5a1 1 0 0 1 1 1v1zM4.118 4 4 4.059V13a1 1 0 0 0 1 1h6a1 1 0 0 0 1-1V4.059L11.882 4H4.118zM2.5 3V2h11v1h-11z"
              /></svg
          ></b-button>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
// import drag from "./drag.vue";
export default {
  data: function() {
    return {
      colorList: [
        {
          name: "orange",
          color: "#f29640",
        },
        {
          name: "blue",
          color: "#b5d0f3",
        },
        {
          name: "yellow",
          color: "#fdf568",
        },
        {
          name: "green",
          color: "#00cc66",
        },
        {
          name: "red",
          color: "#ff6666",
        },
        {
          name: "pink",
          color: "#ff99aa",
        },
        {
          name: "purple",
          color: "#c266ff",
        },
        {
          name: "wheat",
          color: "#fdf9bf",
        },
      ],
      postits: [
        {
          text: "事件風暴",
          color: "yellow",
          pos: { x: 20, y: 0 }, //pos = position
        },
        {
          text: "事件風暴",
          color: "yellow",
          pos: { x: 20, y: 400 },
        },
      ],
      nowId: -1,
      mousePos: {
        x: 0,
        y: 0,
      },
      startMousePos: {
        x: 0,
        y: 0,
      },
    };
  },
  watch: {
    mousePos() {
      if (this.nowId != -1) {
        let nowPostit = this.postits[this.nowId];
        nowPostit.pos.x = this.mousePos.x - this.startMousePos.x;
        nowPostit.pos.y = this.mousePos.y - this.startMousePos.y;
      }
    },
  },

  methods: {
    // 滑鼠移動的控制
    onmousemove(evt) {
      this.mousePos = { x: evt.pageX, y: evt.pageY };
    },
    onmouseup() {
      this.nowId = -1;
    },
    // 就是以前的 selectid
    onmousedown(evt, id) {
      console.log(id);
      let isBlock = evt.srcElement.classList.contains("block");
      let isBtn = evt.srcElement.classList.contains("btn");
      if (!isBlock && !isBtn) {
        this.nowId = id;
        this.startMousePos = {
          x: evt.offsetX,
          y: evt.offsetY,
        };
      } else {
        this.nowId = -1;
      }
    },
    getColor(name) {
      return this.colorList.find((o) => o.name == name);
    },
    postitCss(p) {
      return {
        left: p.pos.x + "px",
        top: p.pos.y + "px",
        fontSize: (120 - 5) / p.text.length - 5 + "px",
        backgroundColor: this.colorList.find((o) => o.name == p.color).color,
      };
    },
    addPostit() {
      // 推資料進去 firebase 給後端參考
      this.postits.push({
        text: "文字",
        color: "yellow",
        pos: { x: 200 + Math.random() * 200, y: 200 + Math.random() * 200 },
      });
    },
    setText(pid) {
      let text = prompt("清輸入新的文字", this.postits[pid].text);
      if (text) {
        this.postits[pid].text = text;
      }
    },
  },
};
</script>

<style>
html,
body,
#app {
  /* background-color: #3e3a39; */
  overflow: auto;
  /* 使用捲軸 */
  width: 100%;
  height: 100%;
}

.postit {
  cursor: pointer;
  width: 120px;
  height: 120px;
  font-size: calc(1250px / 4 - 12px);
  display: flex;
  justify-content: center;
  align-items: center;
  letter-spacing: 5px;
  font-weight: 500;
  box-shadow: 5px 5px 10px rgba(0, 0, 0, 0.4);
  position: absolute;
  transition: background 0.5s;
}
.block {
  width: 15px;
  height: 15px;
  background-color: #fff;
  display: inline-block;
}

.datalist {
  /* border: 1px solid black; */
  position: fixed;
  right: 20px;
  top: 40px;
  width: 185px;
}

/* .text {
  pointer-events: none;
  用了就點不到便利貼
} */
.buttonList .btn {
  /* width: 20px;
  height: 20px; */
  padding: 0px;
  padding-left: 5px;
  padding-right: 5px;
  padding-bottom: 3px;
}

.btnDown {
  margin-top: 5px;
}

.deleteButton {
  padding: 0px;
  /* padding-left: 5px;
  padding-right: 5px;
  padding-bottom: 3px; */
}

.colorList {
  position: absolute;
  bottom: -30px;
  display: flex;
  flex-direction: row;
}

.buttonList {
  position: absolute;
  /* bottom: -40px; */
  left: 130px;
  display: flex;
  flex-direction: column;
}

.fade-enter-active,
.fade-leave-active {
  transition: 0.5s;
}

.fade-enter,
.fade-leave-to {
  opacity: 0;
  transform: scale(0.1);
}
</style>
