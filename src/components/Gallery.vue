<template>
    <div>
      <template v-if="!images"> Loading... </template>
      <template v-else>
        <hooper :itemsToShow="7" :centerMode="true" pagination="no">
          <slide v-for="(image, index) in images" :key="index" :index="index">
            <img
              class="image"
              :src="image"
              @click="openGallery(index)"
              @mousedown="onMouseDown"
              @mouseup="onMouseUp"
              @mousemove="onMouseMove"
            />
          </slide>
  
          <hooper-navigation slot="hooper-addons"></hooper-navigation>
        </hooper>
  
        <GalleryModal
          v-if="galleryIndex !== null"
          :index="galleryIndex"
          :images="images"
          @close="closeGallery"
        />
      </template>
    </div>
  </template>
  
  
  <script>
  import { Hooper, Slide, Navigation as HooperNavigation } from "hooper";
  import GalleryModal from "./GalleryModal";
  import "hooper/dist/hooper.css";
  
  export default {

    components: {
      Hooper,
      Slide,
      HooperNavigation,
      GalleryModal,
    },
    computed: {
      images() {
        return Array.from(Array(20)).map(
          (_, index) => `https://picsum.photos/800/600?random=${index}`
        );
      },
    },
    data() {
      return {
        galleryIndex: null,
        mouseDown: false,
        dragging: false,
      };
    },
    methods: {
      openGallery(index) {
        if (this.dragging) {
          return;
        }
  
        this.galleryIndex = index;
      },
      closeGallery() {
        this.galleryIndex = null;
      },
      onMouseDown() {
        this.mouseDown = true;
      },
      onMouseUp() {
        setTimeout(() => {
          this.mouseDown = false;
          this.dragging = false;
        }, 300); // click tilt time
      },
      onMouseMove() {
        if (!this.mouseDown || this.dragging) {
          return;
        }
  
        this.dragging = true;
      },
    },
  };
  </script>
  
  <style scoped>
  .hooper-slide {
    display: flex;
    align-items: center;
    justify-content: center;
    padding: 3px;
  }
  
  .hooper >>> .hooper-navigation button {
    background: white;
  }
  
  .hooper >>> .hooper-navigation .is-disabled {
    display: none;
  }
  
  .image {
    display: inline-block;
    max-width: 100%;
    max-height: 100%;
    height: auto;
    vertical-align: middle;
  }
  </style>
  
  
  