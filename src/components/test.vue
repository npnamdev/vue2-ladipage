<template>
  <div>
    <MainLayout>
      <template v-slot:header="{ toggleBox, isBoxHidden }">
        <HeaderComponent :toggleBox="toggleBox" :isBoxHidden="isBoxHidden"> 
          
        </HeaderComponent>
      </template>

      <template v-slot:sidebarLeft>
        <SidebarLeft />
      </template>

      <template v-slot:content="{ startDrag, isFloating, boxStyle, isBoxHidden, toggleFloating, toggleBox }">
        <div class="wp-content" :class="{ 'content-expanded': isBoxHidden || isFloating }">
          <div id="content" :class="{ 'content-full': isBoxHidden && isFloating }">
            content
          </div>
          <SidebarRight :startDrag="startDrag" :isFloating="isFloating" :boxStyle="boxStyle" :isBoxHidden="isBoxHidden" :toggleFloating="toggleFloating" :toggleBox="toggleBox">
   
          </SidebarRight>
        </div>
      </template>
    </MainLayout>


    <br><hr>
             <!-- <div id="blocks"></div> -->
            <!-- <div id="traits-container"></div> -->
            <!-- <div class="layers-container"></div> -->
            <!-- <div id="gjs"></div>
            <div id="style-manager-container"></div>
<div id="nam"></div>
            <div id="devElement"></div> -->

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

export default {
  name: "GrapesEditor",
  components: {
    MainLayout,
    HeaderComponent,
    SidebarLeft,
    SidebarRight
  },
  data() {
    return {
      isDragging: false,
      isVisible: false,
      x: 10,
      y: 0,
      offsetX: 0,
      offsetY: 0,
    };
  },
  created() {
    this.y = window.innerHeight / 2 - 190;
  },
  methods: {
    toggle() {
      this.isVisible = !this.isVisible;
    },
    startDrag(event) {
      this.isDragging = true;
      this.offsetX = event.clientX - this.x;
      this.offsetY = event.clientY - this.y;
      document.addEventListener("mousemove", this.drag);
      document.addEventListener("mouseup", this.endDrag);
    },
    endDrag() {
      this.isDragging = false;
      document.removeEventListener("mousemove", this.drag);
      document.removeEventListener("mouseup", this.endDrag);
    },
    drag(event) {
      if (this.isDragging) {
        this.x = event.clientX - this.offsetX;
        this.y = event.clientY - this.offsetY;
      }
    },
    handleClickOutside(event) {
      const dropdown = this.$refs.dropdown;
      if (dropdown && !dropdown.contains(event.target)) {
        this.isVisible = false;
      }
    },
  },
  beforeDestroy() {
    document.removeEventListener("mousedown", this.handleClickOutside);
  },
  mounted() {
    document.addEventListener("mousedown", this.handleClickOutside);
    this.editor = grapesjs.init({
      container: "#gjs",
      fromElement: true,
      width: "auto",
      storageManager: {
        type: "local",
        autosave: true,
      },
      deviceManager: {
        appendTo: "#device-container",
        devices: [
          { id: "desktop", name: "Desktop", width: "" },
          { id: "tablet", name: "Tablet", width: "768px", widthMedia: "992px" },
          {
            id: "mobilePortrait",
            name: "Mobile portrait",
            width: "320px",
            widthMedia: "575px",
          },
        ],
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
      // blockManager: {
      //   appendTo: "#blocks",
      // },
      // layerManager: {
      //   appendTo: ".layers-container",
      // },
      // selectorManager: {
      //   appendTo: ".styles-container",
      // },
      // traitManager: {
      //   appendTo: "#traits-container",
      // },
      // styleManager: {
      //   appendTo: "#style-manager-container",
      //   sectors: [
      //     {
      //       name: "General",
      //       open: false,
      //       buildProps: [
      //         "float",
      //         "display",
      //         "position",
      //         "top",
      //         "right",
      //         "left",
      //         "bottom",
      //       ],
      //     },
      //     {
      //       name: "Dimension",
      //       open: false,
      //       buildProps: [
      //         "width",
      //         "height",
      //         "max-width",
      //         "min-height",
      //         "margin",
      //         "padding",
      //       ],
      //     },
      //     {
      //       name: "Typography",
      //       open: false,
      //       buildProps: [
      //         "font-family",
      //         "font-size",
      //         "font-weight",
      //         "letter-spacing",
      //         "color",
      //         "line-height",
      //         "text-align",
      //         "text-shadow",
      //       ],
      //     },
      //     {
      //       name: "Decorations",
      //       open: false,
      //       buildProps: [
      //         "border-radius-c",
      //         "background-color",
      //         "border-radius",
      //         "border",
      //         "box-shadow",
      //         "background",
      //       ],
      //     },
      //     {
      //       name: "Extra",
      //       open: false,
      //       buildProps: ["opacity", "transition", "perspective", "transform"],
      //       properties: [
      //         {
      //           type: "slider",
      //           property: "opacity",
      //           defaults: 1,
      //           step: 0.01,
      //           max: 1,
      //           min: 0,
      //         },
      //       ],
      //     },
      //   ],
      // },

      panels: {
        appendTo: "#panel-container",
        el: ".gjs-pn-panels",
        defaults: [
          {
            id: "panel-devices",
            el: ".panel__devices",
            buttons: [
              {
                id: "device-desktop",
                label: "D",
                command: "set-device-desktop",
                active: true,
                togglable: false,
              },
              {
                id: "device-mobile",
                label: "M",
                command: "set-device-mobile",
                togglable: false,
              },
            ],
          },
        ],
      },
    });
    
    const devElement = document.getElementById("devElement");
    const demo = document.querySelectorAll('.gjs-sm-property');
    console.log(demo);
    demo.forEach((element) => {
      devElement.appendChild(element);
    });

    const newElements = document.querySelectorAll(
      ".gjs-pn-options, .gjs-pn-devices-c"
    );
    const parentElement = document.getElementById("nam");

    newElements.forEach((element) => {
      parentElement.appendChild(element);
    });

    const newBlocks = document.querySelectorAll(".gjs-block");
    const parentElement2 = document.getElementById("nam2");

    newBlocks.forEach((element) => {
      parentElement2.appendChild(element);
    });

    const parentElement3 = document.getElementById("nam3");
    const canvarDemo = document.querySelector("#gjs");
    parentElement3.appendChild(canvarDemo);

    console.log(canvarDemo);


    // const devicesWp = document.getElementById("devicesWp");
    // const devicesBtn = document.querySelectorAll(
    //   ".gjs-pn-devices-c"
    // );

    // console.log(devicesBtn);
    // devicesWp.appendChild(devicesBtn);

    // devicesBtn.forEach((element) => {
    //   devicesWp.appendChild(element);
    // });



    // this.editor.Panels.addPanel({
    //   id: "basic-actions",
    //   el: ".panel__basic-actions",
    //   buttons: [
    //     {
    //       active: true,
    //       className: "btn-toggle-borders",
    //       label: "<u>B</u>",
    //       command: "sw-visibility", // Built-in command
    //     },
    //     {
    //       id: "export",
    //       className: "btn-open-export",
    //       label: "Exp",
    //       command: "export-template",
    //       context: "export-template", // For grouping context of buttons from the same panel
    //     },
    //     {
    //       id: "show-json",
    //       className: "btn-show-json",
    //       label: "JSON",
    //       context: "show-json",
    //       command(editor) {
    //         editor.Modal.setTitle("Components JSON")
    //           .setContent(
    //             `<textarea style="width:100%; height: 250px;">
    //         ${JSON.stringify(editor.getComponents())}
    //       </textarea>`
    //           )
    //           .open();
    //       },
    //     },
    //     {
    //       id: "preview",
    //       className: "fa fa-eye",
    //       label: "Preview",
    //       command: "preview",
    //       context: "preview",
    //       attributes: { title: "Preview" },
    //     },
    //     {
    //       id: "fullscreen",
    //       className: "fa fa-arrows-alt",
    //       command: "fullscreen",
    //       context: "fullscreen",
    //       label: "Fullscreen",
    //       attributes: { title: "Fullscreen" },
    //     },
    //     {
    //       id: "export-template",
    //       label: "Export",
    //       className: "fa fa-code",
    //       command: "export-template",
    //       attributes: { title: "View code" },
    //     },
    //     {
    //       id: "undo",
    //       label: "undo",
    //       className: "fa fa-code",
    //       command() {
    //         um.undo();
    //       },
    //     },
    //     {
    //       id: "redo",
    //       label: "redo",
    //       className: "fa fa-code",
    //       command() {
    //         um.redo();
    //       },
    //     },
    //     {
    //       id: "clear",
    //       label: "clear",
    //       className: "fa fa-code",
    //       command() {
    //         um.clear();
    //       },
    //     },
    //   ],
    // });

    // const um = this.editor.UndoManager;

    // this.editor.setDragMode("absolute");

    // const bm = this.editor.BlockManager;
    // this.editor.on("load", () => {
    //   this.editor.BlockManager.render([
    //     bm.get("column1").set("category", ""),
    //     bm.get("column2").set("category", ""),
    //     bm.get("column3").set("category", ""),
    //     bm.get("text").set("category", ""),
    //     bm.get("image").set("category", ""),
    //   ]);
    // });
  },
  destroyed() {
    this.editor.destroy();
  },
};
</script>

<style >
@import url("https://fonts.googleapis.com/css2?family=Mulish:ital,wght@0,200..1000;1,200..1000&display=swap");

/* width */
::-webkit-scrollbar {
  width: 10px;
}

/* Track */
::-webkit-scrollbar-track {
  background: #ffffff;
}

/* Handle */
::-webkit-scrollbar-thumb {
  background: #e7dfdf;
}

.devicesWp {
  display: flex;
  border: 1px solid;
}

.gjs-one-bg {
  background: #ffffff !important;
}

.gjs-cv-canvas {
  width: 100%;
  z-index: 1000;
}

.gjs-two-color {
  color: #fff !important;
}

.gjs-four-color-h:hover {
  color: #c5c52f !important;
}

.gjs-block {
  display: flex;
  justify-content: center;
  align-items: center;
  transition: all 0.1s ease-in-out;
  min-height: 85px;
}

.gjs-block__media {
  margin-bottom: 8px;
}

.gjs-block:hover {
  border: 1px solid #c5c52f;
}



.gjs-pn-btn {
  display: flex;
  justify-content: center;
  align-items: center;
  margin: 0;
}

.gjs-pn-btn svg {
  width: 19px;
}

.gjs-pn-buttons {
  display: flex;
  gap: 8px;
}

.gjs-pn-buttons span:hover svg {
  color: #c5c52f;
}

.gjs-four-color {
  color: #c5c52f !important;
}

.gjs-clm-tags {
  height: 200px;
}

.gjs-block__media{
  width: 30px;
}

.gjs-block__media svg{
  width: 30px;
  color: #000!important;
}
.gjs-block-label {
   color: #000!important;
}

.gjs-pn-options {
  position: static;
}

.gjs-pn-devices-c {
  position: static;
}

.gjs-pn-views {
  position: static;
}

.gjs-pn-commands {
  display: none;
}

#nam {
  position: relative;
  height: 100%;
  display: flex;
  gap: 300px;
}

#nam .gjs-one-bg{
  background-color: #fff!important;
}
#nam svg{
  color: #000!important;
  width: 17px;
}

.gjs-pn-devices-c,
.gjs-pn-options {
  height: 100%;
  display: flex;
  align-items: center;
}

.copntainer-nam {
  display: flex;
  gap: 5px;
  height: 100vh;
  overflow: hidden;
}

#nam2 {
  width: 250px;
  display: flex;
  flex-wrap: wrap;
  height: 100%;
  overflow: auto;
}

#nam3 {
  width: 100%;
  height: 100%;
}

.gjs-cv-canvas {
  position: static;
}

.dev-block {
  width: 250px;
}

.gjs-pn-views-container {
  display: none !important;
}

.graperjs-cusstom {
  border: 1px solid yellow;
  margin-top: 100px;
}

#gjs{
  height: 100%;
  width: 100%;
}

/* width */
#gjs::-webkit-scrollbar {
  width: 5px!important;
}

/* Track */
#gjs::-webkit-scrollbar-track {
  background: #f1f1f1!important;
  border-radius: 5px;
}

/* Handle */
#gjs::-webkit-scrollbar-thumb {
  background: #f0eded!important;
}


.gjs-editor.gjs-one-bg{
  background: #ffffff!important;
}

/*  */

.gjs-radio-item-label, .gjs-sm-sector-label, .gjs-input-unit, #gjs-sm-input-holder{
  color: black!important;
  font-weight: bold;
  font-size: 14px;
  letter-spacing: 0px;
}

.gjs-sm-sector-title{
  background-color: #fff;
  height: 45px;
  border-bottom: 1px solid #e9e9e9;;
}

.gjs-sm-sector-caret svg{
  color: black;
}


.gjs-layer-title{
  color: black;
  font-weight: bold;
  height: 30px;

}
</style>