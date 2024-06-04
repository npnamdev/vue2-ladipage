<template>
  <div id="wrapper" ref="fullscreenElement">
    <slot
      class="header"
      name="header"
      :toggleBox="toggleBox"
      :isBoxHidden="isBoxHidden"
      :openFullscreen="openFullscreen"
    />
    <slot name="sidebarLeft" />
    <button id="reset-btn">
      <EyeOff class="icon" />
    </button>
    <slot
      name="content"
      :startDrag="startDrag"
      :isFloating="isFloating"
      :boxStyle="boxStyle"
      :isBoxHidden="isBoxHidden"
      :toggleFloating="toggleFloating"
      :toggleBox="toggleBox"
    />
  </div>
</template>

<script>
import { EyeOff } from "lucide-vue";

export default {
  components: { EyeOff },
  data() {
    return {
      isFloating: false,
      isBoxHidden: false,
      position: { top: 0, right: 0 },
      dragStart: { x: 0, y: 0 },
      boxStart: { top: 0, right: 0 },
      initialPosition: { top: 0, right: 0 },
    };
  },
  computed: {
    boxStyle() {
      if (this.isFloating) {
        return {
          top: `${this.position.top}px`,
          right: `${this.position.right}px`,
          height: "80vh",
          width: "250px",
          position: "absolute",
          transform: "translateY(-50%)",
        };
      } else {
        return {
          top: "0px",
          right: "0px",
          height: "100vh",
          width: "250px",
          position: "relative",
        };
      }
    },
  },
  methods: {
    openFullscreen() {
      const elem = this.$refs.fullscreenElement;
      if (elem.requestFullscreen) {
        elem.requestFullscreen();
      } else if (elem.webkitRequestFullscreen) {
        elem.webkitRequestFullscreen();
      } else if (elem.msRequestFullscreen) {
        elem.msRequestFullscreen();
      }
    },
    toggleFloating() {
      if (this.isFloating) {
        this.position.top = this.initialPosition.top;
        this.position.right = this.initialPosition.right;
      } else {
        this.initialPosition.top = 0;
        this.initialPosition.right = 0;
        this.position.top = window.innerHeight / 2;
        this.position.right = 20;
      }
      this.isFloating = !this.isFloating;
    },
    toggleBox() {
      this.isBoxHidden = !this.isBoxHidden;
    },
    startDrag(event) {
      if (this.isFloating) {
        this.dragStart.x = event.clientX;
        this.dragStart.y = event.clientY;
        this.boxStart.top = this.position.top;
        this.boxStart.right = this.position.right;
        document.addEventListener("mousemove", this.onDrag);
        document.addEventListener("mouseup", this.stopDrag);
      }
    },
    onDrag(event) {
      const deltaX = this.dragStart.x - event.clientX;
      const deltaY = event.clientY - this.dragStart.y;
      this.position.top = this.boxStart.top + deltaY;
      this.position.right = this.boxStart.right + deltaX;
    },
    stopDrag() {
      document.removeEventListener("mousemove", this.onDrag);
      document.removeEventListener("mouseup", this.stopDrag);
    },
  },
};
</script>


<style lang="scss" scoped>
#wrapper {
  #reset-btn {
    width: 25px;
    height: 25px;
    background-color: #0881fc;
    position: absolute;
    left: 0;
    top: 0;
    z-index: 10000;
    display: none;
    border: 0;
    outline: 0;
    border-radius: 3px;
    justify-content: center;
    align-items: center;
    cursor: pointer;

    &:hover {
      background-color: #080cfc;
    }

    &.active {
      display: flex;
    }

    .icon {
      width: 15px;
      color: #fff;
    }
  }
}
#wp-content {
  display: grid;
  grid-template-columns: auto 250px;

  &.active {
    grid-template-columns: auto;
    #content {
      height: 100vh;
    }
  }

  #content {
    height: calc(100vh - 50px);
    border-right: 1px solid #e9e9e9;
    background-color: #fff;
  }

  &.content-expanded {
    grid-template-columns: auto;
  }
}
</style>


