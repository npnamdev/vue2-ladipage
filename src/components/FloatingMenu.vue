<template>
  <div class="floating-menu" :style="{ bottom: buttonPosition.bottom + 'px', right: buttonPosition.right + 'px' }" ref="menu">
    <div v-if="menuItems.length > 1">
      <ul :class="['menu-items', menuDirection, isMenuVisible && 'active']">
        <li v-for="(item, index) in menuItems" :key="index" @click="closeMenu()">
          <a :href="item.url">
            <ion-icon :name="item.icon"></ion-icon>
          </a>
        </li>
      </ul>
      <button class="menu-button" @click="toggleMenu" :class="isMenuVisible && 'active'">
        <ion-icon class="menu-icon" name="add-outline"></ion-icon>
      </button>
    </div>
    <a class="menu-button" v-else href="#">
        <ion-icon name="chatbubble-outline"></ion-icon>
    </a>
  </div>
</template>

<script>
export default {
  props: {
    initialMenuVisible: {type: Boolean, default: false},
    initialMenuItems: { type: Array, default: () => []},
    initialMenuDirection: { type: String, default: "above"},  // above or left
    initialButtonPosition: {type: Object, default: () => ({ bottom: 30, right: 50 })},
  },
  data() {
    return {
      isMenuVisible: this.initialMenuVisible,
      menuDirection: this.initialMenuDirection,
      buttonPosition: this.initialButtonPosition,
      menuItems: this.initialMenuItems,
    };
  },
  mounted() { document.addEventListener('click', this.handleClickOutside);},
  beforeDestroy() { document.removeEventListener('click', this.handleClickOutside);},
  methods: {
    toggleMenu() { this.isMenuVisible = !this.isMenuVisible;},
    closeMenu() { this.isMenuVisible = false;},
    setMenuDirection(direction) { this.menuDirection = direction;},
    setButtonPosition(bottom, right) { this.buttonPosition.bottom = bottom; this.buttonPosition.right = right;},
    handleClickOutside(event) {
      if (this.$refs.menu && !this.$refs.menu.contains(event.target)) { this.isMenuVisible = false;}
    },
  },
};
</script>

<style lang="scss" scoped>
.floating-menu {
  position: fixed;
  display: flex;
  flex-direction: column;
  align-items: center;
  z-index: 100000;
  justify-content: center;

  .menu-button {
    width: 40px;
    height: 40px;
    border-radius: 50%;
    border: none;
    outline: none;
    display: flex;
    justify-content: center;
    align-items: center;
    cursor: pointer;
    background-color: #fff;
    box-shadow: 0px 0px 4px #00000026;
    transition: all 0.3s ease-in-out;

    &:hover{
      background-color: #f1e9e9;
    }
    
    &.active {
      transform: rotate(225deg);
    }

    .menu-icon {
      font-size: 20px;
    }
  }

  .menu-items {
    display: flex;
    position: absolute;
    gap: 8px; 
    opacity: 0;
    transition: all 0.3s ease-in-out;
    &.active {
      opacity: 1;
    }

    &.above {
      flex-direction: column;
      bottom: 50px; 
    }

    &.left {
      flex-direction: row;
      right: 50px;
    }

    li {
      width: 38px;
      height: 38px;
      border-radius: 50%;
      background-color: #fff;
      box-shadow: 0px 0px 4px rgba(0, 0, 0, 0.15);
      display: flex;
      justify-content: center;
      align-items: center;
      cursor: pointer;

      &:hover{
        background-color: #f1e9e9;
      }

      a{
        display: flex;
        color: black;
      }
    }
  }
}
</style>


