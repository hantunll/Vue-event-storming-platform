<template>
  <div>
    <div>
      <b-navbar type="dark" variant="dark">
        <b-navbar-nav>
          <b-nav-item href="#">Home</b-nav-item>

          <!-- Navbar dropdowns -->
          <b-nav-item-dropdown text="Project" right>
            <b-dropdown-item href="#">p1</b-dropdown-item>
            <b-dropdown-item href="#">p2</b-dropdown-item>
            <b-dropdown-item href="#">p3</b-dropdown-item>
            <b-dropdown-item href="#">p4</b-dropdown-item>
          </b-nav-item-dropdown>

          <b-nav-item-dropdown text="User" right>
            <b-dropdown-item href="#">Account</b-dropdown-item>
            <b-dropdown-item href="#">Settings</b-dropdown-item>
          </b-nav-item-dropdown>
        </b-navbar-nav>
      </b-navbar>
    </div>
    <drag v-for="(p, pid) in boundingLine" :key="pid"></drag>
    <!-- 記得改abc -->
    <div class="datalist1">
      <b-button class="addbtn" @click="addBoundingLine">+ 新增匡線</b-button>
      <div v-for="(p, pid) in boundingLine" :key="pid" :style="dragCss(p)">
        <!-- problem -->
        <b-form-input :title="p.text"></b-form-input>
        <b-button class="delbtn" @click="boundingLine.splice(pid, 1)"
          >刪除</b-button
        >
      </div>
    </div>
    <div>
      <postits></postits>
    </div>
  </div>
</template>

<script>
import drag from "./components/drag.vue";
import postits from "./components/postits.vue";

export default {
  name: "App",
  components: {
    drag,
    postits,
  },
  // data: function() {
  //   return {
  //     boundingContext:1,
  //   };
  // },
  data: function() {
    return {
      boundingLine: [
        {
          text: "標題",
          index: 0,
          pos: { x: 250, y: 250 },
        },
      ],
    };
  },
  methods: {
    addBoundingLine() {
      this.boundingLine.push({
        index: 0,
        pos: { x: 400 + Math.random() * 200, y: 400 + Math.random() * 200 },
      });
      // console.log(222);
    },
    dragCss(p) {
      return {
        left: p.pos.x + "px",
        top: p.pos.y + "px",
        padding: "10px",
      };
    },
  },
};
</script>

<style>
.addbtn {
  /* position: absolute; */
  /* right: 100px;
  top: 300px; */
  width: 120px;
  margin-left: 30px;
  z-index: 999;
}
.delbtn {
  margin-left: 50px;
  z-index: 999;
}

.datalist1 {
  padding-top: 20px;
  /* border: 10px solid yellow; */
  position: fixed;
  right: 20px;
  top: 280px;
  width: 185px;
  z-index: 999;
}
/* .bg {
  background-color: rgb(243, 242, 242);
} */
</style>
