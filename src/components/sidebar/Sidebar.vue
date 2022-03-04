<script>
import SidebarLink from "./SidebarLink";
import { collapsed, toggleSidebar, sidebarWidth } from "./state";

export default {
  props: {},
  components: { SidebarLink },
  setup() {
    return { collapsed, toggleSidebar, sidebarWidth };
  },
};
</script>

<template>
  <div class="sidebar" :style="{ width: sidebarWidth }">
    <h1>
      <span v-if="collapsed">
        <div>L</div>
        <div>C</div>
      </span>
      <span v-else><img src="https://i.ibb.co/jJBpCkF/Logo-removebg.png" class="side-img">
</span>
    </h1>
    <div class="navBar mt-5">
    <SidebarLink class="navi" to="/" icon="fas fa-home">Home</SidebarLink>
    <SidebarLink class="navi" to="/products" icon="fas fa-shopping-cart">Products</SidebarLink>
    <SidebarLink class="navi" to="/profile" icon="fas fa-user">Profile</SidebarLink>
    <SidebarLink class="navi" to="/register" icon="fas fa-address-card">Register</SidebarLink>
    <SidebarLink class="navi" to="/login" icon="fas fa-sign-in-alt">Login</SidebarLink>
    <SidebarLink class="navi" @click="logout" :to="{ name: 'Login' }" icon="fas fa-sign-out-alt">Log out</SidebarLink>
    </div>

    <span
      class="collapse-icon"
      :class="{ 'rotate-180': collapsed }"
      @click="toggleSidebar"
    >
      <i class="fas fa-angle-double-left" />
    </span>
  </div>
</template>

<style>
:root {
  --sidebar-bg-color: #f8ad9d;
  --sidebar-item-hover: #f4978e;
  --sidebar-item-active: #f4978e;
}
</style>

<style scoped>
.sidebar {
  color: white;
  background-color: var(--sidebar-bg-color);
  float: left;
  position: fixed;
  z-index: 1;
  top: 0;
  left: 0;
  bottom: 0;
  padding: 0.5em;

  transition: 0.3s ease;

  display: flex;
  flex-direction: column;
}
.navi{
margin-top: 20px !important;
}

.sidebar h1 {
  height: 2.5em;
}

.side-img {
  width: 135px;
}

.collapse-icon {
  position: absolute;
  bottom: 0;
  padding: 0.75em;

  color: rgba(255, 255, 255, 0.7);

  transition: 0.2s linear;
}

.rotate-180 {
  transform: rotate(180deg);
  transition: 0.2s linear;
}
@media (max-width:400px) {
  .collapse-icon {
    display: none !important;
  }
}
</style>
