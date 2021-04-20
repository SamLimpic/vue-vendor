<template>
  <header class="container-fluid">
    <div class="row justify-content-start align-items-center bg-dark">
      <div class="col-1 text-center">
        <img
          class="img-fluid w-50 py-2"
          alt="Vue logo"
          src="./assets/logo.png"
        />
      </div>
      <div class="col-9 text-light">
        <h1>Welcome to Your Vue.js Vending Machine</h1>
      </div>
    </div>
  </header>

  <main class="container">
    <div class="row justify-content-around my-3">
      <div id="sell" class="col-4 p-3">
        <div id="patron">
          <div class="row justify-content-around my-1 text-center">
            <div class="col-10 px-0 bg-dark rounded">
              <div
                class="card shadow bg-danger py-3"
                :style="{ width: `${state.health}%` }"
              ></div>
            </div>
          </div>
          <div class="row justify-content-around my-3 text-center">
            <div class="col-10 px-0 bg-dark rounded">
              <div
                class="card shadow bg-primary py-3"
                :style="{ width: `${state.mana}%` }"
              ></div>
            </div>
          </div>
          <div class="row justify-content-around my-3 text-center">
            <div class="col-10 px-0 bg-dark rounded">
              <div
                class="card shadow bg-success py-3"
                :style="{ width: `${state.stamina}%` }"
              ></div>
            </div>
          </div>
        </div>
        <div
          class="row justify-content-around my-3 text-center align-items-center"
        >
          <div class="col-6 pb-5 mb-3">
            <div id="patron-img" class="text-center">
              <button class="btn btn-warning text-light my-3 align-middle">
                <h5 class="mt-1">
                  <strong>GOLD: {{ state.money }} </strong>
                </h5>
              </button>
              <img
                v-if="
                  state.health > 66 || state.mana > 66 || state.stamina > 66
                "
                class="w-100"
                src="./assets/player.png"
                alt=""
              />
              <img
                v-else-if="
                  state.health > 33 || state.mana > 33 || state.stamina > 33
                "
                class="w-100"
                src="./assets/player-66.png"
                alt=""
              />
              <img
                v-else-if="
                  state.health <= 33 || state.mana <= 33 || state.stamina <= 33
                "
                class="w-100"
                src="./assets/player-33.png"
                alt=""
              />
            </div>
          </div>
          <div class="col-6">
            <button class="btn btn-danger my-3 w-75" @click="sell(0, 'health')">
              <h6 class="mt-1">Sell Blood</h6>
            </button>
            <button class="btn btn-primary my-3 w-75" @click="sell(1, 'mana')">
              <h6 class="mt-1">Make Gold</h6>
            </button>
            <button
              class="btn btn-success my-3 w-75"
              @click="sell(2, 'stamina')"
            >
              <h6 class="mt-1">Hard Labor</h6>
            </button>
          </div>
        </div>
      </div>
      <div id="buy" class="col-8 p-3">
        <div class="row justify-content-around text-center">
          <div class="col-3 p-3 m-3">
            <button class="btn btn-danger" @click="buy(0, 'health')">
              <h4>Health Potion</h4>
            </button>
          </div>
          <div class="col-3 p-3 m-3">
            <button class="btn btn-primary" @click="buy(1, 'mana')">
              <h4>Mana Potion</h4>
            </button>
          </div>
          <div class="col-3 p-3 m-3">
            <button class="btn btn-success" @click="buy(2, 'stamina')">
              <h4>Stamina Potion</h4>
            </button>
          </div>
        </div>
        <div id="shop" class="row justify-content-around">
          <div
            class="col-3 card shadow bg-dark text-light text-center p-3 m-3"
            v-for="potion in state.potions"
            :key="potion.name"
          >
            <img class="mb-3" :src="potion.img" alt="" />
            <h3>
              <u>
                <strong> {{ potion.cost }} </strong> Gold
              </u>
            </h3>
            <h4>
              <strong> {{ potion.stock }} </strong> Left!
            </h4>
          </div>
        </div>
      </div>
    </div>
  </main>

  <footer class="container-fluid fixed-bottom bg-dark">
    <div class="row text-center text-light py-2">
      <div class="col">
        <h5>
          Made with
          <i class="fas fa-filter px-1" style="transform: scaleY(-1)"></i> by
          Samwise
        </h5>
      </div>
    </div>
  </footer>
</template>

<script>
import { reactive } from "vue";
export default {
  setup() {
    const state = reactive({
      money: 0,
      health: 100,
      mana: 100,
      stamina: 100,
      potions: [
        {
          name: "Health",
          cost: 20,
          img: require("./assets/health.png"),
          stock: 5,
          potency: 10,
        },
        {
          name: "Mana",
          cost: 15,
          img: require("./assets/mana.png"),
          stock: 5,
          potency: 10,
        },
        {
          name: "Stamina",
          cost: 10,
          img: require("./assets/stamina.png"),
          stock: 5,
          potency: 10,
        },
      ],
    });
    return {
      state,
      buy(num, type) {
        if (
          state.money >= state.potions[num].cost &&
          state.potions[num].stock != 0 &&
          state[type] < 100
        ) {
          state.potions[num].stock -= 1;
          state.money -= state.potions[num].cost;
          state[type] += state.potions[num].potency;
        }
      },
      sell(num, type) {
        if (state[type] > 15) {
          state[type] -= 10;
          state.potions[num].stock++;
          state.money += 15;
        }
      },
    };
  },
};
</script>

<style>
body {
  display: flex;
  flex-direction: column;
  min-height: 100vh;
}

main {
  flex-grow: 1;
}

.bg-shop {
  background: url(./assets/shop-bg.png) no-repeat center center;
  background-size: cover;
  background-position: bottom;
  height: 100%;
  overflow: hidden;
}
</style>