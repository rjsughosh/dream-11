<template>
  <header class="header">
    <div class="left-header">
      <img src="./../assets/ball.svg" />
      <div>Dream 11</div>
    </div>
    <div class="right-header">
      <img id="userOverlay" src="./../assets/user.svg" />
      <b-popover target="userOverlay" triggers="click" placement="top">
        <div ref="userOverlay" class="user-overlay arrow-box">
          <div class="user-overlay-inner">
            <div class="view-mode" v-if="viewMode">
              <div>{{username}}</div>
              <img @click="viewMode = false" class="edit-icon" src="./../assets/edit.svg" />
            </div>
            <div class="edit-mode" v-if="!viewMode">
              <!-- <b-form-input id="input-user-name" v-model="username" type="text"></b-form-input> -->
              <input class="edit-username" v-model="username" />
              <img @click="viewMode = true" class="edit-icon" src="./../assets/save.svg" />
            </div>
          </div>
        </div>
      </b-popover>
    </div>
  </header>
</template>

<script>
export default {
  data() {
    return {
      username: "Sughosh Jahagirdar",
      viewMode: true,
      overlayOpen: false
    };
  },
  methods: {}
};
</script>

<style scoped>
.header {
  font-size: 3rem;
  background: #2e519f;
  display: flex;
  justify-content: space-between;
  align-items: center;
  padding: 0.5rem;
  color: #fff;
}

.left-header {
  display: flex;
  align-items: center;
}

.right-header {
  position: relative;
  font-size: 1.2rem;
  cursor: pointer;
}

.user-overlay {
  background: #fff;
  color: black;
  border-radius: 5px;
  /* transition: 0.3s all; */
  /* box-shadow: 11px 14px 23px -9px rgba(0, 0, 0, 0.93); */
  padding: 0.5rem;
  font-size: 1.2rem;
}

.user-overlay-inner {
  position: relative;
}

.expand {
  height: auto;
  transition: 0.3s all;
  padding: 1rem;
}

img {
  height: 4rem;
  margin: 0 1.5rem 0 0.5rem;
}

.edit-icon {
  height: 1rem;
  margin: 0;
  margin-left: 0.5rem;
  cursor: pointer;
}

.view-mode,
.edit-mode {
  display: flex;
  align-items: center;
  white-space: nowrap;
}

.edit-username {
  border: none;
  border-bottom: 1px solid #999;
}

.edit-username:focus {
  outline: none;
}

/* .arrow_box {
  position: relative;
  background: #fff;
  border: 4px solid #fff;
}
.arrow_box:after,
.arrow_box:before {
  bottom: 100%;
  left: 50%;
  border: solid transparent;
  content: " ";
  height: 0;
  width: 0;
  position: absolute;
  pointer-events: none;
}

.arrow_box:after {
  border-color: rgba(255, 255, 255, 0);
  border-bottom-color: #fff;
  border-width: 30px;
  margin-left: -30px;
}
.arrow_box:before {
  border-color: rgba(255, 255, 255, 0);
  border-bottom-color: #fff;
  border-width: 36px;
  margin-left: -36px;
} */
</style>>
