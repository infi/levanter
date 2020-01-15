<template>
  <div v-if="show" class="card">
    <header class="card-header">
      <p class="card-header-title">New Card</p>
    </header>
    <div class="card-content">
      <div class="content">
        Cards only apply to this browser.
        <br />Right click to remove a card.
        <br />
        <br />
        <b-field label="Name">
          <b-input type="text" v-model="title" />
        </b-field>
        <b-field label="URL">
          <b-input type="text" v-model="uri" @keypress.native="id = getId()" />
        </b-field>
        <b-field label="ID">
          <p>{{id}}</p>
        </b-field>
      </div>
    </div>
    <footer class="card-footer">
      <a href="#" class="card-footer-item" @click="save">Save</a>
      <a href="#" class="card-footer-item" @click="close">Cancel</a>
    </footer>
  </div>
</template>

<script>
import * as crypto from "crypto";
export default {
  props: ["show"],
  data() {
    return {
      title: "",
      uri: "",
      id: this.getId()
    };
  },
  methods: {
    getId() {
      return crypto.randomBytes(16).toString("hex");
    },
    save() {
      this.$emit("save", { title: this.title, uri: this.uri, id: this.id });
      this.title = "";
      this.uri = "";
      this.id = this.getId();
    },
    close() {
      this.$emit("close");
    }
  }
};
</script>

<style lang="scss">
</style>