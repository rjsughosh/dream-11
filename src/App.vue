<template>
  <div id="app">
    <Header></Header>
    <div class="content">
      <div class="players-list">
        <div class="formation-section each-section-left">
          <div class="each-section-left-header">Choose Team</div>

          <b-form-group
            class="each-form-el add-player-form"
            id="input-group-5"
            label="Team:"
            label-for="input-5"
          >
            <b-form-select
              id="input-5"
              v-model="teamSelected"
              :options="teamOptions"
              v-on:change="changeTeam"
            ></b-form-select>
          </b-form-group>
        </div>
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
                  <!-- <button class="clear-btn" type="reset">×</button> -->
                  <!-- <b-button class="form-btn" type="reset" variant="danger">Reset</b-button> -->
                </b-form-group>
              </div>
            </b-form>
          </div>
        </div>
        <div class="players-list-pl each-section-left">
          <div class="each-section-left-header">Forwards [{{fwds.length}}/{{formation[2]}}]</div>

          <div class="position-section">
            <div
              v-bind:class="{'is-selected':player.selected}"
              v-bind:key="player.number"
              v-for="(player) in team.forwards"
              class="each-player"
            >
              <div
                v-on:click="togglePlayer(player.number,'forwards')"
              >{{player.number}} {{player.name}}</div>
              <button v-on:click="removePlayer(player.number,'forwards')" class="close-button">×</button>
            </div>
          </div>
        </div>
        <div class="players-list-pl each-section-left">
          <div class="each-section-left-header">Midfielders [{{mids.length}}/{{formation[1]}}]</div>

          <div class="position-section">
            <div
              v-bind:class="{'is-selected':player.selected}"
              v-bind:key="player.number"
              v-for="(player) in team.midfielders"
              class="each-player"
            >
              <div
                v-on:click="togglePlayer(player.number,'midfielders')"
              >{{player.number}} {{player.name}}</div>
              <button v-on:click="removePlayer(player.number,'midfielders')" class="close-button">×</button>
            </div>
          </div>
        </div>
        <div class="players-list-pl each-section-left">
          <div class="each-section-left-header">Defenders [{{defs.length}}/{{formation[0]}}]</div>

          <div class="position-section">
            <div
              v-bind:class="{'is-selected':player.selected}"
              v-bind:key="player.number"
              v-for="(player) in team.defenders"
              class="each-player"
            >
              <div
                v-on:click="togglePlayer(player.number,'defenders')"
              >{{player.number}} {{player.name}}</div>
              <button
                v-on:click="removePlayer(player.number,'defenders')"
                class="close-button"
                variant="danger"
              >×</button>
            </div>
          </div>
        </div>
        <div class="players-list-pl each-section-left">
          <div class="each-section-left-header">Goal keepers</div>
          <div class="position-section">
            <div
              v-bind:class="{'is-selected':player.selected}"
              v-bind:key="player.number"
              v-for="(player) in team.goalkeepers"
              class="each-player"
            >
              <div
                v-on:click="togglePlayer(player.number,'goalkeepers')"
              >{{player.number}} {{player.name}}</div>
              <button
                v-on:click="removePlayer(player.number,'goalkeepers')"
                class="close-button"
                variant="danger"
              >×</button>
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
import barca from "./barcelona.json";
import realMadrid from "./real madrid.json";
// import axios from "axios";
export default {
  name: "App",
  components: { Header },
  data() {
    return {
      team: {},
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
      formation: [],
      teamSelected: "",
      teamOptions: [
        { value: "barca", text: "FC Barcelona" },
        { value: "realMadrid", text: "Real Madrid" }
      ]
    };
  },
  computed: {
    fwds: function() {
      return this.team.forwards.filter(i => i.selected === true);
    },
    mids: function() {
      return this.team.midfielders.filter(i => i.selected === true);
    },
    defs: function() {
      return this.team.defenders.filter(i => i.selected === true);
    },
    gks: function() {
      return this.team.goalkeepers.filter(i => i.selected === true);
    }
  },
  methods: {
    togglePlayer(number, position) {
      // this.team[position].filter(p => p.number === number)[0].selected =
      let posIndex;
      if (position == "forwards") posIndex = 2;
      if (position == "midfielders") posIndex = 1;
      if (position == "defenders") posIndex = 0;
      if (position == "goalkeepers") {
        this.team.goalkeepers.map(x => {
          x.selected = !x.selected;
        });
        return;
      }
      // debugger;
      if (this.team[position].filter(p => p.number === number)[0].selected) {
        this.team[position].filter(
          p => p.number === number
        )[0].selected = false;
      } else {
        let temp = this.team[position].filter(x => x.selected);
        if (temp.length < this.formation[posIndex]) {
          this.team[position].filter(
            p => p.number === number
          )[0].selected = true;
        } else {
          console.log("limit reached");
        }
      }
    },

    removePlayer(number, position) {
      let index = this.team[position].indexOf(
        this.team[position].filter(p => p.number === number)[0]
      );
      this.team[position].splice(index, 1);
    },

    onSubmit(evt) {
      evt.preventDefault();

      //check if kit number is unique
      let keys = Object.keys(this.team);
      let allNumbers = [];
      for (let key of keys) {
        let nums = this.team[key].map(x => x.number);
        allNumbers = [...allNumbers, ...nums];
      }
      debugger;
      let bool = allNumbers.indexOf(Number.parseInt(this.form.number));
      if (bool !== -1) {
        console.log("cant add");
        return;
      }
      this.team[this.form.position].unshift({
        name: this.form.name,
        number: this.form.number,
        selected: false,
        position: "forward"
      });
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

      console.log("positionLimit", this.positionLimit);

      this.team.forwards.map(x => (x.selected = false));
      debugger;
      this.team.forwards.map(x => {
        let temp = this.team.forwards.filter(x => x.selected);
        if (temp.length < this.formation[2]) {
          x.selected = true;
        }
      });

      this.team.midfielders.map(x => (x.selected = false));
      this.team.midfielders.map(x => {
        let temp = this.team.midfielders.filter(x => x.selected);
        if (temp.length < this.formation[1]) {
          x.selected = true;
        }
      });

      this.team.defenders.map(x => (x.selected = false));
      this.team.defenders.map(x => {
        let temp = this.team.defenders.filter(x => x.selected);
        if (temp.length < this.formation[0]) {
          x.selected = true;
        }
      });
    },

    changeTeam() {
      this.team = {};
      this.formation = [];
      if (this.teamSelected == "barca") {
        this.team = { ...barca.team };
        this.formation = [...barca.formation];
      }
      if (this.teamSelected == "realMadrid") {
        this.team = { ...realMadrid.team };
        this.formation = [...realMadrid.formation];
      }

      // console.log("this.team", this.team);
      // this.changeFormation();
    }
  },
  created: function() {
    this.team = barca.team;
    this.formation = barca.formation;
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
  border-radius: 50%;
  margin-left: 1rem;
  background: #dc3545;
  font-size: 1.8rem;
  padding: 0px;
  height: 2rem;
  width: 2rem;
  color: #fff;
  display: flex;
  justify-content: center;
  align-items: center;
  border: none;
}

.each-player {
  display: flex;
  justify-content: space-between;
  border: 1px solid #999;
  border-radius: 30px;
  margin: 0.5rem 0.75rem 0.5rem 0;
  padding: 0.4rem;
  align-items: center;
  font-size: 1rem;
  font-weight: normal;
  font-weight: bold;

  /* display: inline; */
  /* min-width: fit-content; */
}

.each-player:hover {
  border: 2px solid #2e519f;
  /* font-weight: bold; */
}

.each-player > div {
  cursor: pointer;
}
.is-selected {
  background: #2e519f;
  /* font-weight: bold; */
  color: #fff;
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
  font-size: 1rem;
  font-weight: normal;
}
.form-btn {
  margin: 0.5rem 0rem 0.5rem 0.85rem;
  width: 4rem;
}

.clear-btn {
  margin: 0.5rem 0rem 0.5rem 0.85rem;
  /* padding: 0.5rem; */
  color: #dc3545;
  font-size: 2rem;
  border: none;
  outline: none;
  display: flex;
  align-items: center;
  justify-content: center;
  /* border-radius: 50%; */
}
.clear-btn:focus {
  outline: none;
}
.clear-btn:hover {
  transform: scale(1.1);
  transition: 0.2s scale;
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
