<template lang="html">
    <!-- <section id="monster" class="container">
        <h2>Monster Health</h2>
        <div class="healthbar">
            <div class="healthbar__value" v-bind:style="monsterBarStyle"></div>
        </div>
    </section> -->
    <!-- <section id="player" class="container">
        <h2>Your Health</h2>
        <div class="healthbar">
            <div class="healthbar__value" :style="playerBarStyle"></div>
        </div>
    </section> -->
    <health-bar name="Monster" :hp="monsterHealth" />
    <health-bar name="Player" :hp="playerHealth" />

    <section id="controls">
        <button @click="attackMonster()">ATTACK</button>
        <button @click="specialAttack()" :disabled="!mayUseSpecialAttack">SPECIAL ATTACK</button>
        <button @click="healPlayer()">HEAL</button>
        <button>SURRENDER</button>
    </section>
    <section id="log" class="container">
        <h2>Battle Log</h2>
        <ul></ul>
    </section>
</template>


<script>
import HealthBar from './components/HealthBarComponent.vue'

export default {
  components: {
    HealthBar
  },
  computed: {
    mayUseSpecialAttack() {
        if(this.currentRound > 0 && this.currentRound % 3 === 0)
            return true;
        else
            return false;
    }
    // monsterBarStyle() {
    //   if(this.monsterHealth < 0)
    //     return 'width: 0%';

    //   return `width: ${this.monsterHealth}%`
    //   //return "width: " + this.monsterHealth + "%"
    // },
    // playerBarStyle() {
    //   if(this.playerHealth < 0)
    //     return 'width: 0%';

    //   return `width: ${this.playerHealth}%`;
    // }
  },
  data() {
    return {
      playerHealth: 100,
      monsterHealth: 100,
      currentRound: 0
    }
  },
  methods: {
    attackMonster() {
      let damage = this.generateRandomNumber(5,12);
      this.monsterHealth -= damage;
      this.attackPlayer();
    },
    attackPlayer() {
      let damage = this.generateRandomNumber(8,15);
      this.playerHealth -= damage;
      this.currentRound++;
    },
    specialAttack() {
      let damage = this.generateRandomNumber(10,25);
      this.monsterHealth -= damage;
      this.attackPlayer();
    },
    healPlayer() {
        let heal = this.generateRandomNumber(8,20);
        this.playerHealth += heal;
        if(this.playerHealth > 100)
            this.playerHealth = 100;
        this.attackPlayer();
    },
    generateRandomNumber(min, max) {
      // Ensure min and max are integers
      min = Math.ceil(min);
      max = Math.floor(max);
      
      // Generate random number between min (inclusive) and max (exclusive)
      return Math.floor(Math.random() * (max - min)) + min;
    }
  }
}
</script>


<style scoped lang="scss">
    @use '../scss/global' as *;

    #controls {
        display: flex;
        flex-direction: row;
        flex-wrap: wrap;
        align-items: center;
        justify-content: center;
    }

    button {
        font: inherit;
        border: 1px solid $color-primary;
        background-color: $color-primary;
        color: $color-white;
        padding: 1rem 2rem;
        border-radius: 12px;
        margin: 1rem;
        width: 12rem;
        cursor: pointer;
        box-shadow: 1px 1px 4px rgba($color-black, 0.26);

        &:focus {
            outline: none;
        }

        &:hover,
        &:active {
            background-color: $color-red;
            border-color: $color-red;
            box-shadow: 1px 1px 8px rgba($color-black, 0.26);
        }

        &:disabled {
            background-color: $color-gray;
            border-color: $color-gray;
            box-shadow: none;
            color: $color-blackLight;
            cursor: not-allowed;
        }
    }

    #log {
        ul {
            list-style: none;
            margin: 0;
            padding: 0;
        }

        li {
            margin: 0.5rem 0;
        }
    }


    .log {
        &--player {
            color: #7700ff;
        }

        &--monster {
            color: #da8d00;
        }

        &--damage {
            color: red;
        }

        &--heal {
            color: green;
        }
    }

</style>