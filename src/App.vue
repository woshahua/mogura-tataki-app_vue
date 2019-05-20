<template>
  <div class="whackamole">
    <h1 class="logo">Whack-a-mole!</h1>
    <button class="start-game" v-on:click="startGame">Start Game</button>
    <div class="counters-container">
      <counter title="Score" v-bind:value="score"/>
      <counter title="High Score" v-bind:value="highscore"/>
      <counter title="Timer" :value="limitTime"/>
    </div>
    <div class="moles-container" :class="{'game-active': isPlaying}">
      <dirt
        :itemId="index"
        :key="index"
        v-for="(item, index) in isActive"
        :isActive="item"
        @update-moe="updateMoe(index)"
      ></dirt>
    </div>
  </div>
</template>

<script>
import dirt from "./mole";
import counter from "./counter";
import { setInterval, clearInterval } from "timers";
import Vue from "vue";

export default {
  name: "App",
  components: {
    dirt,
    counter
  },
  data: function() {
    return {
      limitTime: 20,
      isPlaying: false,
      isFinished: true,
      score: 0,
      highscore: 0,
      isActive: [false, false, false, false]
    };
  },
  methods: {
    updateMoe: function(moeId) {
      if (this.isPlaying) {
        this.score += 1;
        this.stopMoe(moeId);
      }
    },
    startGame: function() {
      if (!this.isPlaying) {
        this.limitTime = 20;
        this.isPlaying = true;
        this.startTimer();
        this.activeRandomMoe();
      }
    },
    startMoe(moeId) {
      this.isActive.splice(moeId, 1, true);
    },
    stopMoe(moeId) {
      this.isActive.splice(moeId, 1, false);
    },
    startTimer() {
      var timer = setInterval(
        function() {
          this.limitTime -= 1;
          if (this.limitTime == 0) {
            // end game
            this.isPlaying = false;
            // update score
            if (this.score > this.highscore) {
              this.highscore = this.score;
              this.score = 0;
            }
            clearInterval(timer);
          }
        }.bind(this),
        1000
      );
    },
    activeRandomMoe() {
      var timer = setInterval(
        function() {
          var moeId = Math.floor(Math.random() * 4);
          this.isActive[moeId] ? this.stopMoe(moeId) : this.startMoe(moeId);
          if (!this.isPlaying) {
            for (let i = 0; i < 4; i++) {
              this.isActive.splice(i, 1, false);
            }
            clearInterval(timer);
          }
        }.bind(this),
        500
      );
    }
  }
};
</script>

<style>
.whackamole {
  font-family: "Bungee", sans-serif;
  max-width: 960px;
  margin: auto;
  text-align: center;
}

.start-game {
  font-family: "Bungee", sans-serif;
  padding: 20px;
  border-radius: 3px;
  border: 0;
  background-color: #52b1d6;
  color: #fff;
  font-size: 1em;
  cursor: pointer;
}

.counters-container {
  display: flex;
  justify-content: space-evenly;
}

.counter {
  border: 1px solid #000;
  margin-top: 20px;
  padding: 20px;
}

.counter h1,
.counter h2 {
  margin: 0;
}

.moles-container {
  margin-top: 20px;
  display: flex;
  justify-content: space-between;
  opacity: 0.5;
  transition: opacity 0.3s ease;
}

.moles-container.game-active {
  opacity: 1;
}

.mole-container {
  width: 160px;
  height: 160px;
  display: inline-block;
  margin: 10px;
  position: relative;
}

.mole-image-container {
  overflow: hidden;
  width: 160px;
  height: 140px;
}

.mole-container img {
  display: block;
  transition: all 0.3s ease;
}

.mole {
  position: relative;
  width: 60%;
  margin: auto;
  cursor: pointer;
}

.mole-container.active .mole {
  /* display: block; */
  top: 0px;
}

.mole-container.inactive .mole {
  top: 200px;
}

.dirt {
  width: 100%;
  margin: auto;
  z-index: 1;
  position: absolute;
  bottom: 0;
}
</style>
