<template>
  <div>
    <MainLayout>
      <template v-slot:header="{ toggleBox, isBoxHidden, openFullscreen }">
        <HeaderComponent
          :toggleBox="toggleBox"
          :isBoxHidden="isBoxHidden"
          :openFullscreen="openFullscreen"
        />
      </template>

      <template v-slot:sidebarLeft>
        <SidebarLeft />
      </template>

      <template
        v-slot:content="{
          startDrag,
          isFloating,
          boxStyle,
          isBoxHidden,
          toggleFloating,
          toggleBox,
        }"
      >
        <div
          id="wp-content"
          :class="{ 'content-expanded': isBoxHidden || isFloating }"
        >
          <div
            id="content"
            :class="{ 'content-full': isBoxHidden && isFloating }"
          >
            <div id="gjs"></div>
          </div>
          <SidebarRight
            :startDrag="startDrag"
            :isFloating="isFloating"
            :boxStyle="boxStyle"
            :isBoxHidden="isBoxHidden"
            :toggleFloating="toggleFloating"
            :toggleBox="toggleBox"
          >
            <!-- <div class="wp">
              <div id="layers-container"></div>
            </div> -->

            <div>
              <div class="tab-buttons">
                <button
                  v-for="tab in tabs"
                  :key="tab"
                  @click="selectTab(tab)"
                  :class="{ active: currentTab === tab }"
                >
                  {{ tab }}
                </button>
              </div>
              <div class="tab-content">
                <div class="item-manage" v-show="currentTab === 'Styles'">
                  <div id="style-manager-container"></div>
                </div>
                <div class="item-manage" v-show="currentTab === 'Properties'">
                  <div id="traits-container"></div>
                  <!-- <div id="dev"></div> -->
                </div>
                <div class="item-manage" v-show="currentTab === 'Layers'">
                  <div id="layers-container"></div>
                </div>
              </div>
            </div>
          </SidebarRight>
        </div>
      </template>
    </MainLayout>


     <div id="dev2"></div>
    <div id="dev"></div>
    <div id="style-manager-container"></div> 

    <div id="nam"></div>
    <div id="traits-container"></div> 
    <div id="style-manager-container"></div>
    <div id="blocks"></div>
  </div>
</template>


<script>
import grapesjs from "grapesjs";
import "grapesjs/dist/css/grapes.min.css";
import "grapesjs/dist/grapes.min.js";
import presetWebpage from "grapesjs-preset-webpage";
import formPlugin from "grapesjs-plugin-forms";
import basicPlugin from "grapesjs-blocks-basic";
import customCodePlugin from "grapesjs-custom-code";
import grapesjsTabs from "grapesjs-tabs";
import grapesjsClick from "grapesjs-click";
import MainLayout from "./MainLayout.vue";
import HeaderComponent from "./HeaderComponent.vue";
import SidebarLeft from "./SidebarLeft.vue";
import SidebarRight from "./SidebarRight.vue";

import "./graperCss/panelManage.scss";
import "./graperCss/blockManage.scss";
import "./graperCss/layerManage.scss";
import "./graperCss/styleManage.scss";
import "./graperCss/main.scss";

export default {
  name: "GrapesEditor",
  data() {
    return {
      tabs: ["Styles", "Properties", "Layers"],
      currentTab: "Styles",
    };
  },
  methods: {
    selectTab(tab) {
      this.currentTab = tab;
    },
  },
  components: { MainLayout, HeaderComponent, SidebarLeft, SidebarRight },
  mounted() {
    document.addEventListener("mousedown", this.handleClickOutside);
    this.editor = grapesjs.init({
      container: "#gjs",
      fromElement: true,
      width: "100%",
      canvasCss: `
        body { background-color: #fff }
        ::-webkit-scrollbar-track { background: #f1f1f1;}
        * ::-webkit-scrollbar-thumb { background: #D8CBCB; border-radius: 10px;}
        * ::-webkit-scrollbar { width: 0px!important; }
      `,
      storageManager: {
        type: "local",
        autosave: true,
      },
      blockManager: {
        appendTo: "#blocks",
      },
      traitManager: {
        appendTo: "#traits-container",
      },
      styleManager: {
        appendTo: "#style-manager-container",
      },
      layerManager: {
        appendTo: "#layers-container",
      },
      plugins: [
        basicPlugin,
        formPlugin,
        presetWebpage,
        customCodePlugin,
        grapesjsTabs,
        grapesjsClick,
      ],
      pluginsOpts: {
        [basicPlugin]: {
          blocks: [
            "column1",
            "column2",
            "column3",
            "column3-7",
            "text",
            "link",
            "image",
            "video",
            "map",
            "quote",
          ],
          addBasicStyle: true,
          category: "",
          labelColumn1: "1 Cột",
          labelColumn2: "2 Cột",
          labelColumn3: "3 Cột",
          labelColumn37: "2 Cột 3/7",
          labelText: "Văn bản",
          labelLink: "Đường dẫn",
          labelImage: "Hình ảnh",
          labelVideo: "Video",
          labelMap: "Bản đồ",
        },
        [formPlugin]: {
          category: "",
        },
        [presetWebpage]: {
          category: "",
        },
        [customCodePlugin]: {
          modalTitle: "Chỉnh sửa code",
        },
        [grapesjsTabs]: {},
      },
    });
    // this.editor.setDragMode("absolute");

    const layerContainer = document.querySelector(".gjs-layers");
    console.log(layerContainer);
    // layerContainer.classList.add('open');

    const newElements = document.querySelectorAll(
      ".gjs-pn-options, .gjs-pn-devices-c"
    );
    const parentElement = document.getElementById("nam");
    newElements.forEach((element) => {
      parentElement.appendChild(element);
    });
    const devElement = document.getElementById("dev");
    const blockElement = document.querySelectorAll(".gjs-block");
    blockElement.forEach((element) => {
      devElement.appendChild(element);
    });
    // const styleElement = document.querySelectorAll(".gjs-sm-property");
    // const devElement2 = document.getElementById("dev2");
    // styleElement.forEach((element) => {devElement2.appendChild(element);});

    const undoBtn = document.getElementById("undo-btn");
    const redoBtn = document.getElementById("redo-btn");
    const clearBtn = document.getElementById("clear-btn");
    const outlineBtn = document.getElementById("outline-btn");
    const codeBtn = document.getElementById("code-btn");
    const previewBtn = document.getElementById("preview-btn");
    const resetBtn = document.getElementById("reset-btn");
    const layoutWpContent = document.getElementById("wp-content");
    const layoutHeader = document.getElementById("header");
    const layoutSidebarRight = document.getElementById("sidebar-right");
    const layoutSidebarLeft = document.getElementById("sidebar-left");

    undoBtn.addEventListener("click", () => {
      this.editor.Commands.run("core:undo");
    });
    redoBtn.addEventListener("click", () => {
      this.editor.Commands.run("core:redo");
    });

    previewBtn.addEventListener("click", () => {
      this.editor.Commands.run("core:preview");
      layoutWpContent.classList.add("active");
      layoutHeader.classList.add("active");
      layoutSidebarRight.classList.add("active");
      layoutSidebarLeft.classList.add("active");
      resetBtn.classList.add("active");
    });

    resetBtn.addEventListener("click", () => {
      this.editor.Commands.stop("core:preview");
      layoutWpContent.classList.remove("active");
      layoutHeader.classList.remove("active");
      layoutSidebarRight.classList.remove("active");
      layoutSidebarLeft.classList.remove("active");
      resetBtn.classList.remove("active");
    });

    clearBtn.addEventListener("click", () => {
      if (confirm("Bạn có chắc chắn muốn xóa Lading page không?")) {
        this.editor.Commands.run("core:canvas-clear");
      }
    });
    clearBtn.addEventListener("click", () => {
      if (confirm("Bạn có chắc chắn muốn xóa Lading page không?")) {
        this.editor.Commands.run("core:canvas-clear");
      }
    });

    this.editor.Commands.run("core:component-outline");
    let isOutlineActive = true;
    outlineBtn.addEventListener("click", () => {
      if (isOutlineActive) {
        this.editor.Commands.stop("core:component-outline");
        outlineBtn.classList.remove("active");
      } else {
        this.editor.Commands.run("core:component-outline");
        outlineBtn.classList.add("active");
      }
      isOutlineActive = !isOutlineActive;
    });

    codeBtn.addEventListener("click", () => {
      this.editor.Commands.run("core:open-code");
    });
  },
  destroyed() {
    this.editor.destroy();
  },
};
</script>


<style lang="scss">
// .gjs-layer-item{
//   height: 36px;
//   color: black;
//   border-bottom: 1px solid #e7dede;
// }

// .gjs-layer-name{
//   color: black;
//   font-weight: 700;
//   letter-spacing: 0
// }

// @import url("https://fonts.googleapis.com/css2?family=Mulish:ital,wght@0,200..1000;1,200..1000&display=swap");

// .gjs-off-prv{
//   display: none!important;
// }

// .wp-icon {
//   display: flex;
//   width: 100% !important;
//   align-items: center;
//   gap: 30px;
//   border: 1px solid;
// }
// .gjs-pn-btn {
//   margin: 0px;
//   display: flex;
//   justify-content: center;
//   align-items: center;
// }

// .gjs-pn-btn svg {
//   color: #523e3e;
//   width: 18px;
// }

// #nam {
//   display: flex;
//   gap: 50px;
//   display: none;
// }
// .gjs-pn-buttons {
//   display: flex;
//   gap: 7px !important;
// }

// #blocks,
// #style-manager-container {
//   // display: none;
// }

// .gjs-one-bg {
//   background-color: white !important;
// }

// #traits-container {
//   display: none;
// }

// .wp {
//   display: flex;
//   flex-direction: column;
// }

// .gjs-pn-views-container {
//   display: none;
// }

// .gjs-pn-views {
//   display: none;
// }

// .gjs-cv-canvas {
//   width: 100%;
//   top: 0;
// }

// .gjs-selected {
//   outline: 2px solid #f0f4f7 !important;
//   outline-offset: -2px;
// }

// .gjs-pn-panel {
//   position: static;
// }

// .sp-container {
//   top: 200px !important;
// }

// .gjs-pn-panels,
// .gjs-pn-commands {
//   display: none !important;
// }

// .gjs-sm-property {
//   width: 100%;
// }

// .gjs-radio-items {
//   display: grid;
//   grid-template-columns: 1fr 1fr;
//   gap: 5px;
//   background-color: transparent;
// }

// .gjs-radio-item {
//   background-color: white;
//   border: none;
// }
// .gjs-fields,
// .gjs-field {
//   background-color: white;
// }

// .gjs-radio-item-label {
//   border: 1px solid #ebe5e5;
//   border-radius: 3px;
//   font-size: 13px;
//   font-weight: 700;
//   padding: 9px 0px;
// }

// .gjs-field {
//   border: 1px solid #ebe5e5;
//   padding: 6px 12px;
//   width: 100%;
//   border-radius: 4px;
// }

// .gjs-field.gjs-select,
// .gjs-field-integer {
//   padding: 3px 0px;
// }

// .gjs-field.gjs-field-radio {
//   border: none;
//   padding: 0;
// }

// .gjs-sm-label {
//   font-size: 13px;
//   font-weight: 700;
// }

// .gjs-pn-options,
// .gjs-pn-devices-c {
//   position: static;
// }

// .gjs-input-holder input {
//   width: 100%;
// }

// #gjs-sm-input-holder {
//   height: 100%;
// }
// #gjs-sm-input-holder select {
//   padding: 5px 12px;
// }

// .gjs-sm-properties {
//   background-color: #fff;
//   height: 0;
//   border: none !important;
// }

// #dev {
//   display: grid;
//   grid-template-columns: 1fr 1fr;
//   gap: 10px;
//   .gjs-block {
//     margin: 0 !important;
//     width: 100% !important;
//     min-height: auto;
//     padding: 15px 0px;
//     display: flex;
//     gap: 10px;
//     &.gjs-one-bg {
//       background-color: #fff;
//     }
//     .gjs-block__media {
//       margin: 0px auto;
//       width: 25px;
//       display: flex;
//       justify-content: center;
//       align-items: center;
//     }
//     .gjs-block-label {
//       font-weight: bold;
//       letter-spacing: 0px;
//       font-size: 12px;
//     }
//   }
// }

// .btn-toggle-borders svg {
//   background: red !important;
//   color: #fff !important;
// }

// #gjs {
//   height: 100% !important;
// }

// .gjs-cv-canvas {
//   height: 100%;
// }

// #dev, #dev2{
//   display: none;
// }
</style>