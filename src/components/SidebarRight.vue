<template>
  <div
    id="sidebar-right"
    :class="[{ floating: isFloating, hiddenBox: isBoxHidden }]"
    :style="boxStyle"
  >
    <div class="icon-action" v-if="isFloating">
      <span></span>
      <GripHorizontal
        @mousedown="startDrag"
        class="icon icon-drag"
        stroke-width="1.5"
      />
      <div class="wp-icon-toggle">
        <Expand @click.stop="toggleFloating" class="icon" />
        <X @click.stop="toggleBox" class="icon icon-x" />
      </div>
    </div>

    <div class="sidebar-right-head" :class="isFloating && 'active'">
      <span></span>

      <slot name="sidebar-right-title"></slot>

      <span v-if="isFloating"></span>
      <Shrink
        v-if="!isFloating"
        @click.stop="toggleFloating"
        class="icon"
        :class="isFloating && 'active'"
      />
    </div>
    <div class="sidebar-right-content">
      <slot></slot>
    </div>
  </div>
</template>


<script>
import { Shrink, X, GripHorizontal, Expand} from "lucide-vue";
export default {
  props: [
    "startDrag",
    "isFloating",
    "boxStyle",
    "isBoxHidden",
    "toggleFloating",
    "toggleBox",
  ],
  components: { Shrink, X, GripHorizontal, Expand},
};
</script>


<style lang="scss" scoped>
#sidebar-right {
  background-color: rgb(255, 255, 255);
  z-index: 50;
  cursor: pointer;
  overflow: hidden;
  position: relative;
  &.active {
    display: none;
  }

  &.floating {
    border-radius: 5px;
    box-shadow: 0 -1px 38px rgba(0, 0, 0, 0.05), 0 4px 7px rgba(0, 0, 0, 0.12);
  }

  &.hiddenBox {
    max-width: 0px;
  }
}

.sidebar-right-head {
  height: 50px;
  display: flex;
  align-items: center;
  padding: 0px 20px;
  justify-content: space-between;
  border-bottom: 1px solid #e9e9e9;

  .title-secleced {
    display: flex;
    align-items: center;
    justify-content: center;
    gap: 10px;

    &.active {
      .icon {
        display: block;
      }
    }

    .icon {
      display: none;
      color: rgb(66, 62, 62);
      font-size: 15px;
    }

    #selected {
      text-transform: uppercase;
      font-size: 12px;
      font-weight: 700;
    }
  }
}

.sidebar-right-content {
  height: calc(100% - 100px);
  padding: 10px 15px;
  overflow: auto;
  padding-top: 0px;
  padding-bottom: 50px;
  background: #fff;
  &::-webkit-scrollbar {
    width: 0px;
  }
}

.icon-action {
  display: none;
  display: flex;
  justify-content: space-between;
  align-items: center;
  padding-top: 5px;
  padding-right: 10px;
  padding-left: 10px;
  span {
    width: 35px;
  }
  .wp-icon-toggle {
    display: flex;
    gap: 8px;
  }
}

.icon {
  width: 15px;
  color: #c2b4b4;
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
</style>
