<template>
  <section>
    <div class="pageTitle">vue-fast-average-color</div>
    <div class="container">
      <ColorCard title="ColorCard Title" subtitle="Subtitle" :alpha="0.45">
        <img
          src="https://source.unsplash.com/random/600x600"
          class="thumb__img"
          crossorigin="anonymous"
        />
      </ColorCard>
    </div>
  </section>
</template>

<script>
import ColorCard from "./ColorCard";

export default {
  name: "HelloWorld",
  components: {
    ColorCard,
  },
  props: {
    msg: String,
  },
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
h3 {
  margin: 40px 0 0;
}
ul {
  list-style-type: none;
  padding: 0;
}
li {
  display: inline-block;
  margin: 0 10px;
}
a {
  color: #42b983;
}
.pageTitle {
  font-size: 0.7rem;
  letter-spacing: 3px;
  color: #2c3e50;
  text-align: center;
  padding: 0 0 45px;
}
</style>
