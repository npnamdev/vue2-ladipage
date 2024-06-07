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
            <template v-slot:sidebar-right-title>
              <div class="title-secleced" v-show="!isEditComponentName">
                <div id="selected">Thiết lập trang</div>
                <PencilLine @click="handleOpenInput" class="icon" />
              </div>
              <div class="title-secleced-change" v-show="isEditComponentName">
                <input
                  type="text"
                  ref="myInput"
                  v-model="componentName"
                  class="input-edit-component-name"
                  @keyup.enter="updateTagName"
                />
                <Save @click="updateTagName" class="icon" />
              </div>
            </template>

            <div class="tab-buttons">
              <button
                v-for="tab in tabs"
                :key="tab"
                @click="selectTab(tab.type)"
                :class="{ active: currentTab === tab.type }"
              >
                <icon :is="tab.icon" class="icon" stroke-width="1.5" />
                <span>{{ tab.title }}</span>
              </button>
            </div>
            <div class="tab-content">
              <div class="item-manage" v-show="currentTab === 'Styles'">
                <div id="selector-container"></div>
                <div id="style-manager-container"></div>
              </div>
              <div class="item-manage" v-show="currentTab === 'Properties'">
                <div id="traits-container"></div>
                <p class="traits-description" v-if="!component">
                  Hãy chọn một thành phần để chỉnh sửa thuộc tính
                </p>
              </div>
              <div class="item-manage" v-show="currentTab === 'Layers'">
                <div id="layers-container"></div>
              </div>
              <div class="item-manage" v-show="currentTab === 'Blocks'">
                <div id="dev"></div>
                <div id="blocks"></div>
              </div>
            </div>
          </SidebarRight>
        </div>
      </template>
    </MainLayout>
    <div id="nam"></div>
    <!-- <div id="blocks"></div> -->

  </div>
</template>


<script>
import grapesjs from "grapesjs";
import "grapesjs/dist/css/grapes.min.css";
import "grapick/dist/grapick.min.css";

import presetWebpage from "grapesjs-preset-webpage";
import formPlugin from "grapesjs-plugin-forms";
import basicPlugin from "grapesjs-blocks-basic";
import customCodePlugin from "grapesjs-custom-code";
import grapesjsTabs from "grapesjs-tabs";
import grapesjsClick from "grapesjs-click";
import grapesjsTuiImageEditor from "grapesjs-tui-image-editor";
import grapesjsStyleGradient from "grapesjs-style-gradient";
import pluginCountdown from "grapesjs-component-countdown";
import pluginTooltip from "grapesjs-tooltip";
import grapesjsBlocksFlexbox from "grapesjs-blocks-flexbox";
import styleFilter from "grapesjs-style-filter";
import grapesjsStyleBg from "grapesjs-style-bg";
import grapesjsTouch from "grapesjs-touch";
import grapesjsPluginCkeditor from "grapesjs-plugin-ckeditor";
import grapesjsNavbar from "grapesjs-navbar";
import parserPostCSS from "grapesjs-parser-postcss";
import grapesjsTyped from "grapesjs-typed";

import MainLayout from "./MainLayout.vue";
import HeaderComponent from "./HeaderComponent.vue";
import SidebarLeft from "./SidebarLeft.vue";
import SidebarRight from "./SidebarRight.vue";
import {
  PencilRuler,
  Settings,
  Layers,
  LayoutGrid,
  PencilLine,
  Save,
} from "lucide-vue";

import "./graperCss/main.scss";
import "./graperCss/panelManage.scss";
import "./graperCss/blockManage.scss";
import "./graperCss/layerManage.scss";
import "./graperCss/styleManage.scss";
import "./graperCss/trainManage.scss";

export default {
  name: "GrapesEditor",
  data() {
    return {
      tabs: [
        {
          type: "Styles",
          title: "Kiểu",
          icon: PencilRuler,
        },
        {
          type: "Properties",
          title: "Thuộc tính",
          icon: Settings,
        },
        {
          type: "Layers",
          title: "Lớp",
          icon: Layers,
        },
        {
          type: "Blocks",
          title: "Thành phần",
          icon: LayoutGrid,
        },
      ],
      editor: "",
      component: "",
      componentName: "",
      isEditComponentName: false,
      currentTab: "Styles",
    };
  },
  methods: {
    selectTab(tab) {
      this.currentTab = tab;
    },
    updateSelectedComponentName(component) {
      const selected = document.querySelector("#selected");
      if (component.get("custom-name")) {
        selected.innerHTML = `${component.get("custom-name")} (#${
          component.ccid
        })`;
      } else if (component.get("name")) {
        selected.innerHTML = `${component.get("name")} (#${component.ccid})`;
      } else if (component.get("type")) {
        selected.innerHTML = `${component.get("type")} (#${component.ccid})`;
      } else {
        selected.innerHTML = `${component.get("tagName")} (#${component.ccid})`;
      }
    },
    updateTagName() {
      this.component.set("custom-name", this.componentName);
      this.isEditComponentName = false;
    },
    handleOpenInput() {
      this.componentName =
        this.component.get("custom-name") ||
        this.component.get("name") ||
        this.component.get("type") ||
        this.component.get("tagName");
      this.isEditComponentName = true;
      this.$nextTick(() => {
        this.$refs.myInput.select();
      });
    },
  },
  components: {
    MainLayout,
    HeaderComponent,
    SidebarLeft,
    SidebarRight,
    PencilRuler,
    PencilLine,
    Save,
  },
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
        * ::-webkit-scrollbar { width: 0px!important; height: 0px}
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
        // sectors: [
        //   {
        //     name: "Thiết lập chung",
        //     open: true,
        //     buildProps: [
        //       "float",
        //       "display",
        //       "position",
        //       "top",
        //       "right",
        //       "left",
        //       "bottom",
        //     ],
        //   },
        //   {
        //     name: "Bố trí",
        //     open: false,
        //     buildProps: [
        //       "width",
        //       "height",
        //       "max-width",
        //       "min-height",
        //       "margin",
        //       "padding",
        //     ],
        //   },
        //   {
        //     name: "Kiểu chữ",
        //     open: false,
        //     buildProps: [
        //       "font-family",
        //       "font-size",
        //       "font-weight",
        //       "letter-spacing",
        //       "color",
        //       "line-height",
        //       "text-align",
        //       "text-shadow",
        //     ],
        //   },
        //   {
        //     name: "Trang trí",
        //     open: false,
        //     buildProps: [
        //       "border-radius-c",
        //       "background-color",
        //       "border-radius",
        //       "border",
        //       "box-shadow",
        //       "background",
        //     ],
        //   },
        //   {
        //     name: "Thêm",
        //     open: false,
        //     buildProps: ["opacity", "transition", "perspective", "transform"],
        //     properties: [
        //       {
        //         type: "slider",
        //         property: "opacity",
        //         defaults: 1,
        //         step: 0.01,
        //         max: 1,
        //         min: 0,
        //       },
        //     ],
        //   },
        // ],
      },
      layerManager: {
        appendTo: "#layers-container",
        open: true,
        showWrapper: false,
      },
      selectorManager: {
        appendTo: "#selector-container",
        custom: true,
      },
      plugins: [
        basicPlugin,
        formPlugin,
        presetWebpage,
        customCodePlugin,
        grapesjsTabs,
        grapesjsClick,
        grapesjsTuiImageEditor,
        grapesjsStyleGradient,
        pluginCountdown,
        pluginTooltip,
        grapesjsBlocksFlexbox,
        styleFilter,
        grapesjsStyleBg,
        grapesjsTouch,
        grapesjsPluginCkeditor,
        grapesjsNavbar,
        parserPostCSS,
        grapesjsTyped,
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
          category: "Basic",
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
          category: "Basic",
        },
        [presetWebpage]: {
          category: "Basic",
        },
        [customCodePlugin]: {
          category: "Basic",
          modalTitle: "Chỉnh sửa code",
        },
        [grapesjsTabs]: {
          category: "Basic",
        },
        [grapesjsTuiImageEditor]: {
          config: {
            includeUI: {
              initMenu: "filter",
            },
          },
        },
        [grapesjsStyleGradient]: {
          colorPicker: "default",
          grapickOpts: {
            min: 1,
            max: 99,
          },
        },
        [pluginCountdown]: {
          /* options */
        },
        [pluginTooltip]: {
          /* options */
        },
        [grapesjsBlocksFlexbox]: {
          /* options */
        },
        [styleFilter]: {
          /* options */
        },
        [grapesjsStyleBg]: {
          /* options */
        },
        [grapesjsTouch]: {
          /* options */
        },
        [grapesjsPluginCkeditor]: {
          /* options */
        },
        [grapesjsNavbar]: {
          /* options */
        },
        [parserPostCSS]: {
          /* options */
        },
        [grapesjsTyped]: {
          /* options */
        },
      },
      grapickOpts: {},
    });


    // Add the new background-image bulti-in type
    this.editor.StyleManager.addProperty("decorations", {
      extend: "background-image", // <- extend the built-in type
      name: "Gradient Background",
    });

    this.editor.StyleManager.addProperty("extra", { extend: "filter" });
    this.editor.StyleManager.addProperty("extra", {
      extend: "filter",
      property: "backdrop-filter",
    });


    const customIconArr = document.querySelectorAll(".gjs-field-arrows");
    customIconArr.forEach((icon) => {
      icon.innerHTML = `<svg xmlns="http://www.w3.org/2000/svg" width="12" height="12" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round" class="lucide lucide-chevrons-up-down"><path d="m7 15 5 5 5-5"/><path d="m7 9 5-5 5 5"/></svg>`;
    });

    // ==============================================================
    const selectedWp = document.querySelector(".title-secleced");
    this.editor.on("component:selected", (component) => {
      console.log(component);
      if (this.currentTab == "Blocks") {
        this.currentTab = "Styles";
      }

      this.component = component;
      selectedWp.classList.add("active");
      component.on("change:custom-name", () => {
        this.updateSelectedComponentName(component);
      });
      this.updateSelectedComponentName(component);
    });

    // ==============================================================
    // const devElement = document.getElementById("dev");
    // const blockElement = document.querySelectorAll(".gjs-block");
    // blockElement.forEach((element) => {
    //   devElement.appendChild(element);
    // });

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

    this.editor.Commands.run("core:component-outline");
    outlineBtn.classList.add("active");
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



