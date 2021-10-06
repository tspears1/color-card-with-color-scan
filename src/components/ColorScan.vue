<script>
import FastAverageColor from "fast-average-color";

export default {
  render() {
    let slot = "";

    if (this.$scopedSlots.default !== undefined) {
      slot = this.$scopedSlots.default({});
    }

    return slot;
  },

  data() {
    return {
      inline: false,
      swatch: {
        primary: {
          value: "",
          rgb: "",
          rgba: "",
          hex: "",
          hexa: "",
          hsl: "",
          hsla: "",
          isDark: "",
          isLight: "",
        },
        secondary: {},
      },
      fac: "",
    };
  },

  props: {
    src: {
      type: String,
    },
    alpha: {
      type: Number,
      default: 1,
    },
    defaultColor: {
      type: Array,
    },
    ignoredColor: {
      type: Array,
    },
    mode: {
      type: String,
    },
    algorithm: {
      type: String,
    },
    step: {
      type: Number,
    },
  },

  mounted() {
    this.inline = this.$scopedSlots.default !== undefined;
    this.fac = new FastAverageColor();
    this.getSwatch();
  },

  computed: {
    imageSource() {
      return this.src ? this.src : this.$el;
    },

    options() {
      return {
        defaultColor: this.defaultColor,
        ignoredColor: this.ignoredColor,
        mode: this.mode,
        algorithm: this.algorithm,
        step: this.step,
      };
    },
  },

  methods: {
    getSwatch() {
      if (this.src || this.inline) {
        this.fac
          .getColorAsync(this.imageSource, this.options)
          .then((color) => {
            this.swatch.primary.value = color.value;
            this.swatch.primary.rgb = this.toRGB(color.value);
            this.swatch.primary.rgba = this.toRGB(color.value, this.alpha);
            this.swatch.primary.hex = color.hex;
            this.swatch.primary.hexa = this.toHEXA(color.hex);
            this.swatch.primary.hsl = this.toHSL(color.value);
            this.swatch.primary.hsla = this.toHSL(color.value, this.alpha);
            this.swatch.primary.isDark = color.isDark;
            this.swatch.primary.isLight = color.isLight;
            this.shareSwatch();
          })
          .catch((e) => {
            console.error(e);
          });
      } else {
        console.warn("ColorScan: no image source");
      }
    },

    toRGB(value, alpha) {
      const rgb = value.slice(0, 3);

      if (!alpha) {
        return "rgb( " + rgb.join(", ") + " )";
      } else {
        const rgba = [].concat(rgb, alpha);
        return "rgba( " + rgba.join(", ") + " )";
      }
    },

    toHEXA(hex) {
      let alpha = Math.round(this.alpha * 255);
      alpha = alpha.toString(16);
      alpha = alpha.length === 1 ? "0" + alpha : alpha;
      return hex.concat(alpha);
    },

    toHSL(value, alpha) {
      let r = value[0] / 255;
      let g = value[1] / 255;
      let b = value[2] / 255;

      let cmin = Math.min(r, g, b),
        cmax = Math.max(r, g, b),
        delta = cmax - cmin,
        h = 0,
        s = 0,
        l = 0;

      // Calculate hue
      // No difference
      if (delta === 0) h = 0;
      // Red is max
      else if (cmax === r) h = ((g - b) / delta) % 6;
      // Green is max
      else if (cmax === g) h = (b - r) / delta + 2;
      // Blue is max
      else h = (r - g) / delta + 4;

      h = Math.round(h * 60);

      // Make negative hues positive behind 360°
      if (h < 0) h += 360;

      // Calculate lightness
      l = (cmax + cmin) / 2;

      // Calculate saturation
      s = delta === 0 ? 0 : delta / (1 - Math.abs(2 * l - 1));

      // Multiply l and s by 100
      s = +(s * 100).toFixed(1);
      l = +(l * 100).toFixed(1);

      if (!alpha) return "hsl( " + h + ", " + s + "%, " + l + "% )";
      else return "hsla( " + h + ", " + s + "%, " + l + "%, " + alpha + " )";
    },

    shareSwatch() {
      this.$emit("share-swatch", this.swatch);
    },
  },
};
</script>