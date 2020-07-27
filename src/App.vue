<template>
  <div id="app">
    <Header></Header>
    <div class="content">
      <div class="players-list">
        <div class="formation-section each-section-left">
          <div class="each-section-left-header">Choose formation</div>

          <b-form-group
            class="each-form-el add-player-form"
            id="input-group-3"
            label="Formation:"
            label-for="input-3"
          >
            <b-form-select
              id="input-0"
              v-model="formation"
              :options="formationOptions"
              v-on:change="changeFormation"
            ></b-form-select>
          </b-form-group>
        </div>
        <div class="add-player-wrap each-section-left">
          <div class="each-section-left-header">Add Player</div>
          <div>
            <b-form @submit="onSubmit" @reset="onReset" v-if="formShow">
              <div class="add-player-form">
                <b-form-group
                  class="each-form-el"
                  id="input-group-1"
                  label="Player Name:"
                  label-for="input-1"
                >
                  <b-form-input id="input-1" v-model="form.name" type="text" required></b-form-input>
                </b-form-group>

                <b-form-group
                  class="each-form-el"
                  d="input-group-2"
                  label="Kit Number:"
                  label-for="input-2"
                >
                  <b-form-input
                    class="kit-number"
                    id="input-2"
                    v-model="form.number"
                    type="number"
                    required
                    max="99"
                  ></b-form-input>
                </b-form-group>

                <b-form-group
                  class="each-form-el"
                  id="input-group-3"
                  label="Position:"
                  label-for="input-3"
                >
                  <b-form-select id="input-3" v-model="form.position" :options="positionOptions"></b-form-select>
                </b-form-group>
                <b-form-group class="each-form-el">
                  <b-button class="form-btn add-btn" type="submit" variant="primary">Add</b-button>
                  <b-button class="form-btn" type="reset" variant="danger">Reset</b-button>
                </b-form-group>
              </div>
            </b-form>
          </div>
        </div>
        <div class="players-list-pl each-section-left">
          <div class="each-section-left-header">Forwards</div>

          <div class="position-section">
            <div
              v-bind:class="{'is-selected':player.selected}"
              v-bind:key="player.number"
              v-for="(player, i) in forwards"
              class="each-player"
            >
              <div v-on:click="togglePlayer(i,'forwards')">{{player.number}} {{player.name}}</div>
              <b-button
                v-on:click="removePlayer(i,'forwards')"
                class="close-button"
                variant="danger"
              >×</b-button>
            </div>
          </div>
        </div>
        <div class="players-list-pl each-section-left">
          <div class="each-section-left-header">Midfielders</div>

          <div class="position-section">
            <div
              v-bind:class="{'is-selected':player.selected}"
              v-bind:key="player.number"
              v-for="(player, i) in midfielders"
              class="each-player"
            >
              <div v-on:click="togglePlayer(i,'midfielders')">{{player.number}} {{player.name}}</div>
              <b-button
                v-on:click="removePlayer(i,'midfielders')"
                class="close-button"
                variant="danger"
              >×</b-button>
            </div>
          </div>
        </div>
        <div class="players-list-pl each-section-left">
          <div class="each-section-left-header">Defenders</div>

          <div class="position-section">
            <div
              v-bind:class="{'is-selected':player.selected}"
              v-bind:key="player.number"
              v-for="(player, i) in defenders"
              class="each-player"
            >
              <div v-on:click="togglePlayer(i,'defenders')">{{player.number}} {{player.name}}</div>
              <b-button
                v-on:click="removePlayer(i,'defenders')"
                class="close-button"
                variant="danger"
              >×</b-button>
            </div>
          </div>
        </div>
        <div class="players-list-pl each-section-left">
          <div class="each-section-left-header">Goal keepers</div>
          <div class="position-section">
            <div
              v-bind:class="{'is-selected':player.selected}"
              v-bind:key="player.number"
              v-for="(player, i) in goalkeepers"
              class="each-player"
            >
              <div v-on:click="togglePlayer(i,'goalkeepers')">{{player.number}} {{player.name}}</div>
              <b-button
                v-on:click="removePlayer(i,'goalkeepers')"
                class="close-button"
                variant="danger"
              >×</b-button>
            </div>
          </div>
        </div>
      </div>
      <div class="players-lineup">
        <div class="forwards">
          <div v-bind:key="player.number" v-for="player in fwds">
            <div class="each-player-box" v-if="player.selected">{{player.number}}</div>
          </div>
        </div>
        <div class="mid">
          <div v-bind:key="player.number" v-for="player in mids">
            <div class="each-player-box" v-if="player.selected">{{player.number}}</div>
          </div>
        </div>
        <div class="defence">
          <div v-bind:key="player.number" v-for="player in defs">
            <div class="each-player-box" v-if="player.selected">{{player.number}}</div>
          </div>
        </div>
        <div class="gk">
          <div v-bind:key="player.number" v-for="player in gks">
            <div class="each-player-box" v-if="player.selected">{{player.number}}</div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import Header from "./components/Header";
export default {
  name: "App",
  components: { Header },
  data() {
    return {
      forwards: [
        {
          name: "Lionel Messi",
          number: 10,
          selected: true,
          position: "forward"
        },
        {
          name: "Luis Suarez",
          number: 9,
          selected: true,
          position: "forward"
        },
        {
          name: "Antoine Griezman",
          number: 7,
          selected: false,
          position: "forward"
        },
        {
          name: "Ousmane Dembele",
          number: 11,
          selected: true,
          position: "forward"
        },
        {
          name: "Martin Brathwaite",
          number: 32,
          selected: false,
          position: "forward"
        },
        {
          name: "Francisco Trincao",
          number: 35,
          selected: false,
          position: "forward"
        }
      ],

      midfielders: [
        {
          name: "Frankie De Jong",
          number: 8,
          selected: true,
          position: "midfield"
        },
        {
          name: "Sergio Busquets",
          number: 6,
          selected: true,
          position: "midfield"
        },
        {
          name: "Riqui Puig",
          number: 7,
          selected: true,
          position: "midfield"
        },
        {
          name: "Arturo Vidal",
          number: 22,
          selected: false,
          position: "midfield"
        },
        {
          name: "Pedri Lopez",
          number: 16,
          selected: false,
          position: "midfield"
        }
      ],

      defenders: [
        {
          name: "Sergi Roberto",
          number: 2,
          selected: true,
          position: "defender"
        },
        {
          name: "Gerard Pique",
          number: 3,
          selected: true,
          position: "defender"
        },
        {
          name: "Clement Lenglet",
          number: 5,
          selected: true,
          position: "defender"
        },
        {
          name: "Jordi Alba",
          number: 18,
          selected: true,
          position: "defender"
        },
        {
          name: "Ronald Araujo",
          number: 31,
          selected: false,
          position: "defender"
        }
      ],

      goalkeepers: [
        { name: "Ter Stegen", number: 1, selected: true, position: "gk" },
        { name: "Neto", number: 12, selected: false, position: "gk" }
      ],

      positionLimit: {
        forwards: 3,
        defenders: 4,
        midfielders: 3,
        goalkeepers: 1
      },
      form: {
        name: "",
        number: "",
        position: ""
      },
      positionOptions: [
        { value: "forwards", text: "Forward" },
        { value: "midfielders", text: "Midfield" },
        { value: "defenders", text: "Defence" },
        { value: "goalkeepers", text: "Goalkeeper" }
      ],
      formShow: true,
      positionSelected: null,
      formationOptions: [
        { value: [4, 3, 3], text: "4-3-3" },
        { value: [4, 2, 4], text: "4-2-4" },
        { value: [4, 4, 2], text: "4-4-2" },
        { value: [4, 5, 1], text: "4-5-1" },
        { value: [3, 5, 2], text: "3-5-2" },
        { value: [3, 4, 3], text: "3-4-3" },
        { value: [5, 4, 1], text: "5-4-1" },
        { value: [5, 3, 2], text: "5-3-2" }
      ],
      formation: []
    };
  },
  computed: {
    fwds: function() {
      return this.forwards.filter(i => i.selected === true);
    },
    mids: function() {
      return this.midfielders.filter(i => i.selected === true);
    },
    defs: function() {
      return this.defenders.filter(i => i.selected === true);
    },
    gks: function() {
      return this.goalkeepers.filter(i => i.selected === true);
    }
  },
  methods: {
    togglePlayer(index, position) {
      switch (position) {
        case "forwards": {
          if (this.forwards[index].selected) {
            this.forwards[index].selected = false;
          } else {
            let temp = this.forwards.filter(x => x.selected);
            if (temp.length < this.positionLimit[position]) {
              this.forwards[index].selected = true;
            } else {
              console.log("limit reached");
            }
          }
          break;
        }
        case "midfielders": {
          if (this.midfielders[index].selected) {
            this.midfielders[index].selected = false;
          } else {
            let temp = this.midfielders.filter(x => x.selected);
            if (temp.length < this.positionLimit[position]) {
              this.midfielders[index].selected = true;
            } else {
              console.log("limit reached");
            }
          }
          break;
        }
        case "defenders": {
          if (this.defenders[index].selected) {
            this.defenders[index].selected = false;
          } else {
            let temp = this.defenders.filter(x => x.selected);
            if (temp.length < this.positionLimit[position]) {
              this.defenders[index].selected = true;
            } else {
              console.log("limit reached");
            }
          }
          break;
        }
        case "goalkeepers": {
          if (this.goalkeepers[index].selected) {
            this.goalkeepers[index].selected = false;
          } else {
            let temp = this.goalkeepers.filter(x => x.selected);
            if (temp.length < this.positionLimit[position]) {
              this.goalkeepers[index].selected = true;
            } else {
              console.log("limit reached");
            }
          }
          break;
        }
      }
    },
    removePlayer(index, position) {
      switch (position) {
        case "forwards": {
          this.forwards.splice(index, 1);
          break;
        }
        case "midfielders": {
          this.midfielders.splice(index, 1);

          break;
        }
        case "defenders": {
          this.defenders.splice(index, 1);

          break;
        }
        case "goalkeepers": {
          this.goalkeepers.splice(index, 1);

          break;
        }
      }
    },
    onSubmit(evt) {
      evt.preventDefault();
      switch (this.form.position) {
        case "forwards": {
          this.forwards = [
            {
              name: this.form.name,
              number: this.form.number,
              selected: false,
              position: "forward"
            },
            ...this.forwards
          ];
          break;
        }
        case "midfielders": {
          this.midfielders.unshift({
            name: this.form.name,
            number: this.form.number,
            selected: false,
            position: "midfielders"
          });
          break;
        }
        case "defenders": {
          this.defenders.unshift({
            name: this.form.name,
            number: this.form.number,
            selected: false,
            position: "defenders"
          });
          break;
        }
        case "goalkeepers": {
          this.goalkeepers.unshift({
            name: this.form.name,
            number: this.form.number,
            selected: false,
            position: "goalkeepers"
          });
          break;
        }
      }
      console.log("this.forwards", this.forwards);
      console.log("this.midfielders", this.midfielders);
      console.log("this.defenders", this.defenders);
      console.log("this.goalkeepers", this.goalkeepers);
      // alert(JSON.stringify(this.form));
    },
    onReset(evt) {
      evt.preventDefault();
      // Reset our form values
      this.form.name = "";
      this.form.number = "";
      this.form.position = "";
    },
    changeFormation() {
      this.positionLimit.defenders = this.formation[0];
      this.positionLimit.midfielders = this.formation[1];
      this.positionLimit.forwards = this.formation[2];

      this.forwards.map(x => (x.selected = false));
      this.forwards.map(x => {
        let temp = this.forwards.filter(x => x.selected);
        if (temp.length < this.positionLimit.forwards) {
          x.selected = true;
        }
      });

      this.midfielders.map(x => (x.selected = false));
      this.midfielders.map(x => {
        let temp = this.midfielders.filter(x => x.selected);
        if (temp.length < this.positionLimit.midfielders) {
          x.selected = true;
        }
      });

      this.defenders.map(x => (x.selected = false));
      this.defenders.map(x => {
        let temp = this.defenders.filter(x => x.selected);
        if (temp.length < this.positionLimit.defenders) {
          x.selected = true;
        }
      });
    }
  }
};
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  display: flex;
  flex-direction: column;
  height: 100%;
}

* {
  box-sizing: border-box;
  margin: 0;
  padding: 0;
}

label {
  margin: 0 !important;
}

.content {
  display: flex;
  height: 100%;
  background: #1b9245;
}

.players-list {
  width: 50%;
  background: #eee;
  height: 100%;
  /* /* overflow-y: auto; */
  overflow-y: auto;
  white-space: nowrap;
  max-height: calc(100vh - 88px);
  font-size: 1.4rem;
  font-weight: bold;
}

.players-lineup {
  width: 50%;

  /* background-image: url("./assets/pitch.jpeg"); */
  background: url("./assets/pitch2.jpg") no-repeat center center;

  /* background-size: cover; */
  background-repeat: no-repeat;
  background-size: contain;
  background-position: center center;
  padding: 0 10rem;
  display: flex;
  flex-direction: column;
  justify-content: space-around;
}
.players-list-pl {
  /* display: inline-block; */
  /* display: flex; */
  /* overflow: ;; */
}
.forwards,
.mid,
.defence,
.gk {
  display: flex;
  justify-content: space-around;
  margin: 1rem 0;
  /* min-height: 25%; */
}

.forwards .each-player-box,
.mid .each-player-box,
.defence .each-player-box,
.gk .each-player-box {
  width: 100px;
  height: 100px;

  color: white;
  font-size: 2.5rem;
  display: flex;
  align-items: center;
  justify-content: center;
  background-image: url("./assets/blue.png");
  background-size: cover; /* <------ */
  background-repeat: no-repeat;
  background-position: center center;
}

.close-button {
  border-radius: 50% !important;
  margin-left: 1rem;
}

.each-player {
  display: flex;
  justify-content: space-between;
  border: 1px solid #999;
  border-radius: 30px;
  margin: 0.5rem;
  padding: 0.5rem;
  align-items: center;
  font-size: 1.2rem;
  font-weight: normal;
  /* display: inline; */
  /* min-width: fit-content; */
}

.each-player > div {
  cursor: pointer;
}
.is-selected {
  border: 2px solid #2e519f;
  font-weight: bold;
}
.position-section {
  display: flex;
  flex-wrap: wrap;
}

.add-player-wrap {
  display: flex;
  flex-direction: column;
}

.each-form-el {
  display: flex;
  margin: 0 1.5rem 0 0;
  align-items: center;
}

.form-control,
.custom-select {
  margin-left: 0.5rem;
}
.add-player-form {
  display: flex;
  font-size: 1.2rem;
  font-weight: normal;
}
.form-btn {
  margin: 0.5rem;
  width: 4rem;
}

.kit-number {
  width: 4rem !important;
}

.add-btn {
  background: #2e519f !important;
}

.form-group {
  margin-bottom: 0 !important;
}

.each-section-left {
  padding: 1rem;
  /* margin-bottom: 1rem; */
}

.each-section-left-header {
  margin: 0.5rem 0;
}

@media only screen and (max-width: 760px) {
  .content {
    flex-direction: column;
  }
  .players-lineup {
    width: 100%;
  }
  .players-list {
    width: 100%;
  }
}
</style>
