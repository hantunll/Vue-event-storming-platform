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
    <div class="drag">
      <drag></drag>
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
};
</script>

<style></style>
