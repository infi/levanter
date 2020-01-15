<template>
  <div class="home">
    <section :class="'hero is-fullheight is-'+getHomeColor()+' is-bold'">
      <div class="hero-body">
        <div class="container">
          <h1 class="title">{{time}}</h1>
          <b-input placeholder="Search" v-model="searchterm" @keypress.enter.native="search"></b-input>
          <br />
          <h5 class="is-size-6 has-text-weight-bold">Favorites</h5>
          <div class="favs">
            <span class="favs">
              <span v-for="card in cards" :key="card.id">
                <div class="card is-favorite">
                  <div class="card-content">
                    <p
                      class="subtitle"
                      @click="goto(card.uri)"
                      @click.right.prevent="del(card.id)"
                    >{{card.title}}</p>
                  </div>
                </div>
              </span>
            </span>
            <span class="plus" v-if="!showNew" @click="toggleShow">+</span>
            <CardPrompt :show="showNew" @save="addCard($event)" @close="toggleShow" />
          </div>
        </div>
      </div>
    </section>
    <div class="invis">Levanter v0.0.1</div>
  </div>
</template>

<style lang="scss">
.invis {
  position: absolute;
  bottom: 0;
  left: 0;
  margin: 8px;
  background-color: none;
  width: max-content;
  color: rgba(0, 0, 0, 0.25);
}

.plus {
  background-color: rgb(0, 0, 0, 0.25);
  width: max-content;
  color: white;
  height: max-content;
  //padding: 2.5px 5px 4px;
  padding: 0 5px;
  //margin-top: 5px;
  cursor: pointer;
  user-select: none;
  font-size: 1.25em;
}

.is-favorite {
  width: max-content;
  margin-right: 5px;
  cursor: pointer;
  .subtitle {
    font-size: 1em;
  }
  .card-content {
    padding: 5px;
  }
}

.favs {
  display: flex;
}

* {
  transition: 0.3s;
}
</style>
  
<script>
import cfg from "../config";
import CardPrompt from "../components/CardPrompt";

export default {
  name: "home",
  data() {
    return {
      searchterm: "",
      time: "",
      cards: [],
      showNew: false
    };
  },
  methods: {
    search() {
      document.location.href = `${cfg.searchBaseURI}${encodeURI(
        this.searchterm
      )}`;
    },
    getHomeColor() {
      return cfg.colorTheme;
    },
    toggleShow() {
      this.showNew = !this.showNew;
    },
    addCard(input) {
      this.cards.push(input);
      this.toggleShow();
      this.save();
    },
    goto(uri) {
      window.location.href = uri;
    },
    save() {
      localStorage.setItem("levanter-cards", JSON.stringify(this.cards));
    },
    del(id) {
      this.cards.splice(
        this.cards.findIndex(i => {
          return i.id === id;
        }),
        1
      );
      this.save();
    }
  },
  mounted() {
    this.cards = JSON.parse(localStorage.getItem("levanter-cards")) || [];
    this.time = new Date().toLocaleString(cfg.locale, {
      day: "numeric",
      month: "short",
      year: "numeric",
      weekday: "long",
      second: "numeric",
      minute: "numeric",
      hour: "numeric"
    });
    setInterval(() => {
      this.time = new Date().toLocaleString(cfg.locale, {
        day: "numeric",
        month: "short",
        year: "numeric",
        weekday: "long",
        second: "numeric",
        minute: "numeric",
        hour: "numeric"
      });
    }, 1000);
  },
  components: {
    CardPrompt
  }
};
</script>

