<template>
  <header id="header">
    <div class="header-left">
      <img src="../assets/logo.svg" class="logo" />
      <ul class="icon-header-action">
        <li class="active">
          <Plus class="icon active" :strokeWidth="1.5" />
        </li>
        <li>
          <Layers class="icon" :strokeWidth="1.5" />
        </li>
        <li>
          <LayoutPanelTop class="icon" :strokeWidth="1.5" />
        </li>
        <li>
          <Settings class="icon" :strokeWidth="1.5" />
        </li>
      </ul>
    </div>

    <ul class="header-center">
      <li id="desktop">
        <MonitorDot class="icon" :strokeWidth="1.5" />
      </li>
      <li id="tablet">
        <Tablet class="icon" :strokeWidth="1.5" />
      </li>
      <li id="mobile">
        <TabletSmartphone class="icon" :strokeWidth="1.5" />
      </li>
    </ul>

    <div class="header-right">
      <ul class="list-item">
        <li
          v-for="(icon, index) in icons"
          :key="index"
          @click="icon.method"
          :id="icon.id"
        >
          <component :is="icon.component" class="icon" :strokeWidth="1.5" />
        </li>
      </ul>
      <div class="list-btn">
        <button id="preview-btn" class="btn-view" >Xem trước</button>
        <button class="btn-export">Lưu và xuất bản</button>
      </div>
      <SlidersVertical
        class="icon"
        @click="toggleBox"
        :strokeWidth="1.5"
        :class="isBoxHidden && 'active'"
      />
    </div>
  </header>
</template>

<script>
import {
  Undo2,
  Redo2,
  Eraser,
  Grid2X2,
  FileScan,
  ScanEye,
  SlidersVertical,
  PaintBucket,
  Palette,
  GitCompare,
  Layers,
  Wrench,
  Expand,
  LayoutPanelTop,
  Plus,
  Settings,
  MonitorDot,
  Tablet,
  TabletSmartphone,
} from "lucide-vue";

export default {
  props: ["toggleBox", "isBoxHidden", "openFullscreen"],
  components: {
    Undo2,
    Redo2,
    Eraser,
    Grid2X2,
    FileScan,
    ScanEye,
    SlidersVertical,
    PaintBucket,
    Palette,
    GitCompare,
    Layers,
    Wrench,
    Expand,
    LayoutPanelTop,
    Plus,
    Settings,
    MonitorDot,
    Tablet,
    TabletSmartphone,
  },
  data() {
    return {
      icons: [
        { component: Undo2, method: this.handleAirplay, id: "undo-btn" },
        { component: Redo2, method: this.handleCircleHelp, id: "redo-btn" },
        { component: Eraser, method: this.handlePaintRoller, id: "clear-btn" },
        { component: Grid2X2, method: this.handleBug, id: "outline-btn" },
        { component: Expand, method: this.openFullscreen },
        { component: FileScan, method: this.handleBook, id: "code-btn" },
        // { component: ScanEye, method: this.handleSettings, id: "preview-btn" },

        // { component: Layers, method: this.handleLayers },
        // { component: PaintBucket, method: this.handlePaintBucket, id: "reset-btn2" },
        // { component: Palette, method: this.handlePalette },
        // { component: GitCompare, method: this.handleGitCompare },
        // { component: Wrench, method: this.handleWrench },
      ],
    };
  },
  methods: {
    handleAirplay() {
      console.log("Airplay icon clicked");
      // Add your custom method logic here
    },
    handleCircleHelp() {
      console.log("CircleHelp icon clicked");
      // Add your custom method logic here
    },
    handlePaintRoller() {
      console.log("PaintRoller icon clicked");
      // Add your custom method logic here
    },
    handleBug() {
      console.log("Bug icon clicked");
      // Add your custom method logic here
    },
    handleBook() {
      console.log("Book icon clicked");
      // Add your custom method logic here
    },
    handleSettings() {
      console.log("Settings icon clicked");
      // Add your custom method logic here
    },
    handlePaintBucket() {
      console.log("PaintBucket icon clicked");
      // Add your custom method logic here
    },
    handlePalette() {
      console.log("Palette icon clicked");
      // Add your custom method logic here
    },
    handleGitCompare() {
      console.log("GitCompare icon clicked");
      // Add your custom method logic here
    },
    handleLayers() {
      console.log("Layers icon clicked");
      // Add your custom method logic here
    },
    handleWrench() {
      console.log("Wrench icon clicked");
      // Add your custom method logic here
    },
    handlePencilRuler() {
      console.log("PencilRuler icon clicked");
      // Add your custom method logic here
    },
  },
};
</script>

<style lang="scss" scoped>
#header {
  height: 50px;
  background-color: #fff;
  display: flex;
  align-items: center;
  justify-content: space-between;
  padding: 0px 20px;
  border-bottom: 1px solid #e9e9e9;

  &.active {
    display: none;
  }

  .header-left {
    display: flex;
    gap: 30px;
  }

  .header-right {
    display: flex;
    align-items: center;
    gap: 20px;
  }

  .logo {
    width: 30px;
  }
  .icon-header-action {
    display: flex;
    justify-content: center;
    align-items: center;
    gap: 10px;
    li {
      display: flex;
      align-items: center;
      cursor: pointer;
      width: 25px;
      height: 25px;
      display: flex;
      justify-content: center;
      align-items: center;
      border-radius: 4px;
      &:nth-child(2) {
        margin-left: 3px;
      }
      &.active {
        background-color: #f1f3f4;
        .icon {
          color: #1A56DB;
        }
      }
    }
  }

  .header-center {
    display: flex;
    justify-content: center;
    align-items: center;
    gap: 10px;
    li {
      display: flex;
      align-items: center;
      cursor: pointer;
      width: 25px;
      height: 25px;
      display: flex;
      justify-content: center;
      align-items: center;
      border-radius: 4px;
      &.active {
        background-color: #f1f3f4;
        .icon {
          color: #1A56DB;
        }
      }
    }
  }

  .list-item {
    margin: 0;
    display: flex;
    list-style: none;
    align-items: center;
    gap: 10px;
    // display: none;
    li {
      display: flex;
      align-items: center;
      cursor: pointer;
      width: 28px;
      height: 28px;
      display: flex;
      justify-content: center;
      align-items: center;
      border-radius: 4px;
      &.active {
        background-color: #f1f3f4;

        .icon {
          color: #1A56DB;
        }
      }
    }
  }
  .icon {
    width: 16px;
    color: #000000;
    cursor: pointer;
    &:hover {
      color: #1A56DB;
    }
    &.active {
      color: #1A56DB;
    }
  }

  .list-btn {
    display: flex;
    align-items: center;
    justify-content: center;
    button {
      padding: 7px 10px;
      border: none;
      font-size: 12.5px;
      &:hover {
        color: #1A56DB;
      }
    }

    .btn-export {
      background-color: #1A56DB;
      border-radius: 3px;
      color: #fff;
      font-weight: 500;
      padding: 7px 15px;
      margin-left: 5px;
      &:hover {
        color: #fff;
      }
    }
  }
}
</style>
