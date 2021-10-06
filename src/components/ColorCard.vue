<template>
  <div class="colorCard">
    <a href="#" class="thumb__link">
      <div class="thumb__inner">
        <div class="thumb__imageContainer">
          <ColorScan @share-swatch="storeColorScanSwatch" :alpha="alpha">
            <slot></slot>
          </ColorScan>
          <div
            class="thumb__screen"
            :style="{ background: swatch.primary.rgba }"
            v-if="swatch"
          ></div>
          <div class="thumb__text">
            <div class="thumb__textWrapper" :style="{ color: text }">
              <div class="title">{{ title }}</div>
              <div class="subtitle">{{ subtitle }}</div>
            </div>
          </div>
        </div>
      </div>
    </a>
    <div
      class="bg__color"
      :style="{ color: text, backgroundColor: swatch.primary.rgb }"
      v-if="swatch"
    >
      Primary color is:
      <br />
      {{ swatch.primary.rgb }} <br />
      {{ swatch.primary.rgba }} <br />
      {{ swatch.primary.hex }} <br />
      {{ swatch.primary.hexa }} <br />
      {{ swatch.primary.hsl }} <br />
      {{ swatch.primary.hsla }} <br />
      {{ swatch.primary.isDark ? "Text color: Light" : "Text color: Dark" }}
    </div>
  </div>
</template>

<script>
import ColorScan from "./ColorScan";

export default {
  components: {
    ColorScan,
  },

  data() {
    return {
      swatch: "",
      text: "",
    };
  },

  props: {
    title: {
      type: String,
    },
    subtitle: {
      type: String,
    },
    darkText: {
      type: String,
      default: "#000",
    },
    lightText: {
      type: String,
      default: "#fff",
    },
    alpha: {
      type: Number,
      default: 1,
    },
  },

  watch: {
    swatch(val) {
      this.updateText(val);
    },
  },

  methods: {
    storeColorScanSwatch(val) {
      this.swatch = val;
    },

    updateText(color) {
      this.text = color.primary.isDark ? this.lightText : this.darkText;
    },
  },
};
</script>