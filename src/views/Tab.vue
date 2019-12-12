<template>
  <div class="home">
    <section class="hero is-fullheight is-dark">
      <div class="hero-body">
        <div class="container">
          <h1 class="title">{{time}}</h1>
          <b-input placeholder="Search" v-model="searchterm" @keypress.enter.native="search"></b-input>
        </div>
      </div>
    </section>
    <div class="invis">
      Levanter v0.0.1
    </div>
  </div>
</template>

<script>
import cfg from "@/config.js"

export default {
  name: "tab",
  data() {
    return {
      searchterm: "",
      time: ""
    }
  },
  methods: {
    search() {
      document.location.href = `${cfg.searchBaseURI}${encodeURI(this.searchterm)}`
    }
  },
  mounted() {
    this.time = new Date().toLocaleString(cfg.locale, {
        day: "numeric",
        month: "short",
        year: "numeric",
        weekday: "long",
        second: "numeric",
        minute: "numeric",
        hour: "numeric",
      })
    setInterval(() => {
      this.time = new Date().toLocaleString(cfg.locale, {
        day: "numeric",
        month: "short",
        year: "numeric",
        weekday: "long",
        second: "numeric",
        minute: "numeric",
        hour: "numeric",
      })
    }, 1000);
  }
};
</script>

<style lang="scss" scoped>
.invis {
  position: absolute;
  bottom: 0;
  left: 0;
  margin: 8px;
  color: rgba(255,255,255,.25)
}
</style>
