<template>
  <header id="header">
    <div class="header-left">
      <img src="../assets/logo.svg" class="logo" />
      <ul class="icon-header-action">
        <li class="active">
          <Plus class="icon active" stroke-width="1.5"/>
        </li>
        <li>
          <Layers class="icon" stroke-width="1.5"/>
        </li>
        <li>
          <LayoutPanelTop class="icon" stroke-width="1.5"/>
        </li>
        <li>
          <Settings class="icon" stroke-width="1.5"/>
        </li>
      </ul>
    </div>

    <ul class="header-center">
      <li>
        <MonitorDot class="icon" stroke-width="1.5"/>
      </li>
      <li>
        <Tablet class="icon" stroke-width="1.5"/>
      </li>
    </ul>

    <div class="header-right">
      <ul class="list-item">
        <li v-for="(icon, index) in icons" :key="index" @click="icon.method" :id="icon.id">
          <component :is="icon.component" class="icon" stroke-width="1.5" />
        </li>
      </ul>
      <SlidersVertical class="icon" @click="toggleBox" stroke-width="1.5" :class="isBoxHidden && 'active'"/>
    </div>
  </header>
</template>

<script>
import { RotateCcw, RotateCw, Trash2, Grid2X2, Code, Eye, SlidersVertical, PaintBucket, Palette, GitCompare, Layers, Wrench, Maximize, LayoutPanelTop , Plus, Settings, MonitorDot, Tablet} from "lucide-vue";

export default {
  props: ["toggleBox", "isBoxHidden", "openFullscreen"],
  components: { RotateCcw, RotateCw, Trash2, Grid2X2, Code, Eye, SlidersVertical, PaintBucket, Palette, GitCompare, Layers, Wrench, Maximize, LayoutPanelTop, Plus, Settings, MonitorDot, Tablet},
  data() {
    return {
      icons: [
        { component: RotateCcw, method: this.handleAirplay, id: "undo-btn"},
        { component: RotateCw, method: this.handleCircleHelp, id: "redo-btn"},
        { component: Trash2, method: this.handlePaintRoller, id: "clear-btn"},
        { component: Grid2X2, method: this.handleBug, id: "outline-btn" },
        { component: Maximize, method: this.openFullscreen },
        { component: Code, method: this.handleBook, id: "code-btn" },
        { component: Eye, method: this.handleSettings, id: "preview-btn"},

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
      console.log('Airplay icon clicked');
      // Add your custom method logic here
    },
    handleCircleHelp() {
      console.log('CircleHelp icon clicked');
      // Add your custom method logic here
    },
    handlePaintRoller() {
      console.log('PaintRoller icon clicked');
      // Add your custom method logic here
    },
    handleBug() {
      console.log('Bug icon clicked');
      // Add your custom method logic here
    },
    handleBook() {
      console.log('Book icon clicked');
      // Add your custom method logic here
    },
    handleSettings() {
      console.log('Settings icon clicked');
      // Add your custom method logic here
    },
    handlePaintBucket() {
      console.log('PaintBucket icon clicked');
      // Add your custom method logic here
    },
    handlePalette() {
      console.log('Palette icon clicked');
      // Add your custom method logic here
    },
    handleGitCompare() {
      console.log('GitCompare icon clicked');
      // Add your custom method logic here
    },
    handleLayers() {
      console.log('Layers icon clicked');
      // Add your custom method logic here
    },
    handleWrench() {
      console.log('Wrench icon clicked');
      // Add your custom method logic here
    },
    handlePencilRuler() {
      console.log('PencilRuler icon clicked');
      // Add your custom method logic here
    }
  }
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

  &.active{
    display: none;
  }

  .header-left {
    display: flex;
    gap: 30px;
  }

  .header-right{
    display: flex;
    align-items: center;
    gap: 30px;
  }

  .logo {
    width: 30px;
  }
  .icon-header-action{
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
      &.active{
        background-color: #F1F3F4;
        .icon{
          color: #3b09ee;
        }
      }
    }
  }

  .header-center{
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
      &.active{
        background-color: #F1F3F4;
        .icon{
          color: #3b09ee;
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
      &.active{
          background-color: #F1F3F4;

        .icon{
          color: #3b09ee;
        }
      }
    }
  }
  .icon {
    width: 16px;
    color: #000000;
    cursor: pointer;
    &:hover {
      color: #0881fc;
    }
    &.active {
      color: #0881fc;
    }
  }
}
</style>
