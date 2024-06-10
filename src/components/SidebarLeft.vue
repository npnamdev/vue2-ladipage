<template>
  <div
    id="sidebar-left"
    class="sidebar-left"
    :style="{ top: `${position.top}px`, left: `${position.left}px` }"
  >
    <div class="drag-block" @mousedown="startDrag">
      <GripHorizontal class="icon" />
    </div>
    <ul class="list-menu">
      <li @click="toggleMenu('imageDown')">
        <ImageDown class="icon" :strokeWidth="1.5"/>
        <ul v-if="openMenu === 'imageDown'" class="list-sub-menu">
          <li>menu1</li>
          <li>menu2</li>
          <li>menu3</li>
        </ul>
      </li>

      <li @click="toggleMenu('')">
        <Type class="icon" :strokeWidth="1.5"/>
      </li>

      <li @click="toggleMenu('')">
        <TextQuote class="icon" :strokeWidth="1.5"/>
      </li>

      <li @click="toggleMenu('')">
        <SquareMousePointer class="icon" :strokeWidth="1.5"/>
      </li>

      <li @click="toggleMenu('')">
        <Shapes class="icon" :strokeWidth="1.5"/>
      </li>

      <li @click="toggleMenu('')">
        <Star class="icon" :strokeWidth="1.5"/>
      </li>

      <li @click="toggleMenu('')">
        <Clapperboard class="icon" :strokeWidth="1.5"/>
      </li>

      <li @click="toggleMenu('circlePlus')">
        <CirclePlus class="icon" :strokeWidth="1.5"/>
        <ul v-if="openMenu === 'circlePlus'" class="list-sub-menu">
          <li>menu4</li>
          <li>menu5</li>
          <li>menu6</li>
        </ul>
      </li>
    </ul>
  </div>
</template>


<script>
import { GripHorizontal, ImageDown,Type, TextQuote, SquareMousePointer,Shapes, Star, Clapperboard, CirclePlus } from "lucide-vue";

export default {
  components: { GripHorizontal, ImageDown,Type, TextQuote, SquareMousePointer,Shapes, Star, Clapperboard, CirclePlus },
  data() {
    return {
      position: { top: 0, left: 0 },
      isDragging: false,
      dragStart: { x: 0, y: 0 },
      boxStart: { top: 0, left: 0 },
      boxSize: { width: 45, height: 450 },
      openMenu: null,
    };
  },
  mounted() {
    this.centerVertically();
    this.position.left = 10;
    document.addEventListener("click", this.handleClickOutside);
  },
  beforeDestroy() {
    document.removeEventListener("click", this.handleClickOutside);
  },
  methods: {
    startDrag(event) {
      this.isDragging = true;
      this.dragStart.x = event.clientX;
      this.dragStart.y = event.clientY;
      this.boxStart.top = this.position.top;
      this.boxStart.left = this.position.left;
      document.addEventListener("mousemove", this.onDrag);
      document.addEventListener("mouseup", this.stopDrag);
    },
    onDrag(event) {
      if (this.isDragging) {
        const deltaX = event.clientX - this.dragStart.x;
        const deltaY = event.clientY - this.dragStart.y;
        let newLeft = this.boxStart.left + deltaX;
        let newTop = this.boxStart.top + deltaY;
        const viewportWidth = window.innerWidth;
        const viewportHeight = window.innerHeight;
        newLeft = Math.min(
          Math.max(newLeft, 0),
          viewportWidth - this.boxSize.width
        );
        newTop = Math.min(
          Math.max(newTop, 0),
          viewportHeight - this.boxSize.height
        );
        this.position.left = newLeft;
        this.position.top = newTop;
      }
    },
    stopDrag() {
      this.isDragging = false;
      document.removeEventListener("mousemove", this.onDrag);
      document.removeEventListener("mouseup", this.stopDrag);
    },
    centerVertically() {
      const viewportHeight = window.innerHeight;
      this.position.top = (viewportHeight - this.boxSize.height) / 2;
    },
    toggleMenu(menu) {
      this.openMenu = this.openMenu === menu ? null : menu;
    },
    handleClickOutside(event) {
      const menuItems = document.querySelectorAll(
        ".sidebar-left .list-menu > li"
      );
      const isClickInsideMenu = Array.from(menuItems).some((item) =>
        item.contains(event.target)
      );
      if (!isClickInsideMenu) {
        this.openMenu = null;
      }
    },
  },
};
</script>



<style lang="scss" scoped>
.list-menu {
  display: flex;
  flex-direction: column;
  list-style: none;
  align-items: center;
  padding: 15px 0px;
  gap: 9px;
}

.list-sub-menu {
  position: absolute;
  left: 50px;
  width: 250px;
  top: 0;
  background: #fff;
  box-shadow: 0 -1px 38px rgba(0, 0, 0, 0.05), 0 4px 7px rgba(0, 0, 0, 0.12);
  border-radius: 4px;
  height: 100%;
  list-style: none;
  padding: 20px;
}

.sidebar-left {
  width: 42px;
  background-color: rgb(255, 255, 255);
  position: absolute;
  box-shadow: 0 -1px 38px rgba(0, 0, 0, 0.05), 0 4px 7px rgba(0, 0, 0, 0.12);
  border-radius: 4px;
  z-index: 100;
  &.active{
    display: none;
  }
}

.icon {
  width: 15px;
  color: black;
  cursor: pointer;
  &:hover {
    color: #0881fc;
  }
  &.active {
    color: #0881fc;
  }
  &.icon-drag {
    width: 20px;
    cursor: move;
  }
  &.icon-x {
    width: 17px;
  }
}

.drag-block{
  display: flex;
  justify-content: center;
  align-items: center;
  padding-top: 10px;
  cursor: move;
  .icon{
  cursor: move;
  }
}
</style>
