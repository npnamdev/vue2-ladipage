
<template>
  <div class="circular-menu" :class="{ active: isActive }">
    <div
      v-if="circulaMenu.length > 1"
      class="circular-menu-toggle"
      :class="{ sequential: effects === 'sequential' }"
      @click="toggleMenu"
    >
      <ion-icon name="add-outline"></ion-icon>
    </div>
    <a v-else :href="circulaMenu[0].link" class="circular-menu-toggle">
      <ion-icon :name="circulaMenu[0].icon"></ion-icon>
    </a>
    <li
      v-for="(circulaItem, index) in circulaMenu"
      :key="index"
      :style="'--i: ' + index"
      :class="effects"
      @click="hideMenu"
    >
      <a :href="circulaItem.link">
        <ion-icon :name="circulaItem.icon"></ion-icon>
      </a>
    </li>
  </div>
</template>

<script>
// Nhúng Link Icon CDN:  https://ionic.io/ionicons/usage
export default {
  data() {
    return {
      isActive: false,
      clickDisabled: false,
      disableClickTimeout: 500,
      effects: "sequential", // sequential(default) or simultaneous
      circulaMenu: [
        { icon: "home-outline", link: "#" },
        { icon: "mail-outline", link: "#" },
        { icon: "person-outline", link: "#" },
        { icon: "settings-outline", link: "#" },
        { icon: "key-outline", link: "#" },
        { icon: "videocam-outline", link: "#" },
        { icon: "game-controller-outline", link: "#" },
        { icon: "camera-outline", link: "#" },
      ],
    };
  },
  mounted() {
    document.addEventListener("click", this.handleOutsideClick);
  },
  beforeDestroy() {
    document.removeEventListener("click", this.handleOutsideClick);
  },
  methods: {
    toggleMenu() {
      if (!this.clickDisabled) {
        this.isActive = !this.isActive;
        this.clickDisabled = true;
        setTimeout(() => { this.clickDisabled = false;}, this.disableClickTimeout);
      }
    },
    hideMenu() {
      if (!this.clickDisabled) {
        this.isActive = false;
        setTimeout(() => { this.clickDisabled = false;}, this.disableClickTimeout);
      }
    },
    handleOutsideClick(event) {
      if (!this.clickDisabled && !this.$el.contains(event.target)) {
        this.isActive = false;
        setTimeout(() => { this.clickDisabled = false;}, this.disableClickTimeout);
      }
    },
  },
};
</script>


<style lang="scss">
* {
  padding: 0;
  margin: 0;
  box-sizing: border-box;
}

body {
  display: flex;
  justify-content: center;
  align-items: center;
  min-height: 100vh;
  background: #8460ed;
}

.circular-menu {
  position: fixed;
  width: 180px;
  height: 180px;
  display: flex;
  justify-content: center;
  align-items: center;
  bottom: 10px;
  right: 10px;
  user-select: none;
  z-index: 1000000;
}

.circular-menu li {
  position: absolute;
  left: 0;
  list-style: none;
  transform-origin: 90px;
  transform: rotate(0deg) translateX(68px);
  transition: 0.3s calc(0.1s * var(--i));
}

.circular-menu li.sequential {
  transition: 0.3s calc(0.1s * var(--i));
}

.circular-menu li.simultaneous {
  transition: transform 0.3s, transition-delay 0.3s;
}

.circular-menu li a {
  display: flex;
  justify-content: center;
  align-items: center;
  background-color: #fff;
  width: 40px;
  height: 40px;
  border-radius: 50%;
  transform: rotate(calc(360deg / -8 * var(--i)));
  box-shadow: 0px 0px 4px rgba(0, 0, 0, 0.15);
  color: #111;
  transition: 0.3s;
}
.circular-menu li a:hover {
  background-color: rgb(223, 231, 238);
}

.circular-menu.active li {
  transform: rotate(calc(360deg / 8 * var(--i)));
}

.circular-menu .circular-menu-toggle {
  position: absolute;
  width: 45px;
  height: 45px;
  background-color: #fff;
  display: flex;
  justify-content: center;
  align-items: center;
  z-index: 1000;
  border-radius: 50%;
  cursor: pointer;
  box-shadow: 0px 0px 4px rgba(0, 0, 0, 0.15);
  font-size: 24px;
  transition: transform 1s;
}

.circular-menu .circular-menu-toggle:hover {
  background-color: rgb(223, 231, 238);
}

.circular-menu .circular-menu-toggle.sequential {
  transition: transform 0.3s;
}

.circular-menu.active .circular-menu-toggle {
  transform: rotate(315deg);
}
</style>
