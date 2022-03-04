<script setup>
import ConfettiExplosion from "vue-confetti-explosion";
</script>

<template>
  <Transition>
    <div class="card" :class="{ shake: disabled }">
      <div class="in-progress" v-if="tries < 15">
        <div class="points" v-if="tries < 15">{{ points }} / 15 pontos</div>
        <div class="space"></div>
        <div v-if="!active" class="game">
          <span class="country">{{ country }}</span>
          <input type="text" v-model="guess" @keyup.enter="checkAnswer" /><br />
          <button @click="checkAnswer">Enviar</button>
        </div>
        <div v-else>
          <div v-if="correct" class="game">
            <span class="correct">Correto!</span><br />
            <ConfettiExplosion />
            <button @click="nextQuestion">Próxima</button>
          </div>
          <div v-else class="game">
            <span class="wrong">Oops!</span><br />
            <button @click="nextQuestion">Próxima</button>
          </div>
        </div>
        <div class="space"></div>
      </div>
      <div v-if="tries == 15" class="end-game">
        <span class="ended">Fim do jogo!</span><br />
        <ConfettiExplosion />
        <ConfettiExplosion />
        <span class="ended"> {{ points }} / 15 </span>
      </div>
    </div>
  </Transition>
</template>

<script>
export default {
  name: "CountryCard",

  data() {
    return {
      active: false,
      disabled: false,
      correct: false,
      guess: "",
      points: 0,
      tries: 0,
      // randomColor: _.sample(['#0a9dae', '#ffcf70', '#1dbced', '#3f3a55'])
    };
  },

  props: {
    country: {
      type: String,
    },
    capital: {
      type: String,
    },
  },

  methods: {
    checkAnswer() {
      if (this.guess === this.capital) {
        this.points++;
        this.correct = true;
      } else {
        this.disabled = true;
        this.correct = false;
      }
      this.guess = "";
      this.active = true;
      this.tries++;
    },
    nextQuestion() {
      this.$parent.getRandomInt();
      this.active = false;
    },
  },
};
</script>

<style scoped>
.card {
  background-color: #3f3a55;
  width: 300px;
  height: 400px;
  box-shadow: 20px 20px rgba(0, 0, 0, 0.15);
}

.in-progress {
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
  height: 100%;
}

.points {
  margin-top: 15px;
  font-size: 15px;
  color: white;
  font-weight: 600;
}

.space {
  flex-grow: 1;
}

.country,
.capital,
.correct,
.wrong,
.ended {
  padding: 15px;
  font-size: 35px;
  color: white;
  font-weight: 600;
}

.game {
  display: flex;
  flex-direction: column;
  align-items: center;
}

.game span {
  text-align: center;
  margin-bottom: 25px;
}

.game input[type="text"] {
  border: 1px solid #c1c1c1;
  border-radius: 5px;
  width: 200px;
  height: 20px;
  margin-bottom: 15px;
  font-size: 1.1em;
}

.end-game {
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  height: 100%;
}

.v-enter-active {
  animation: bounce-in 0.5s;
}

.v-leave-active {
  animation: bounce-in 0.5s reverse;
}

@keyframes bounce-in {
  0% {
    transform: scale(0);
  }

  50% {
    transform: scale(1.25);
  }

  100% {
    transform: scale(1);
  }
}

.shake {
  animation: shake 0.82s cubic-bezier(0.36, 0.07, 0.19, 0.97) both;
  transform: translate3d(0, 0, 0);
}

@keyframes shake {
  10%,
  90% {
    transform: translate3d(-1px, 0, 0);
  }

  20%,
  80% {
    transform: translate3d(2px, 0, 0);
  }

  30%,
  50%,
  70% {
    transform: translate3d(-4px, 0, 0);
  }

  40%,
  60% {
    transform: translate3d(4px, 0, 0);
  }
}
</style>
