<template>
<div id="app">
<div class="menu">
        <v-card-title color="#3F51B5">
          <span class="headline" color="#3F51B5">Monster Project</span>

          <div class="flex-grow-1"></div>

          <v-menu>
              <v-btn
                dark
                icon
              >
                <v-icon>mdi-dots-vertical</v-icon>
              </v-btn>
              </v-menu>
              </v-card-title>
</div>

<div class="appWidth">
    <section>
            <h1 align-self-center class="text-center">YOU</h1>
            <div class="healthbar">
                 <v-container>
      <v-progress-linear
      class="healthbar"
                        style="background-color: #B388FF;color: white; margin: 0; "
                        :style="{width: playerHealth + '%'}"
                        :height="30"
    
      >{{ playerHealth }}</v-progress-linear>
    </v-container>
        </div>
        
            <h1 class="text-center">MONSTER</h1>
            <div class="healthbar" text-center>
                          <v-container>
      <v-progress-linear
      class="healthbar"
                        style="background-color: #651FFF;color: white; margin: 0;"
                        :style="{width: monsterHealth + '%'}"
                        :height="30"
    
      >{{ monsterHealth }}</v-progress-linear>
    </v-container>
                    
                    
             </div>
         
       
    </section>
    
    <section class="controls" v-if="!gameIsRunning">
            <v-btn color="#C51162" @click="startGame">START NEW GAME</v-btn>
    </section>
    <section class="controls" v-else>
        <v-btn text color="#880E4F" @click="attack">ATTACK</v-btn>
        <v-btn text color="#FF80AB" @click="specialAttack">SPECIAL ATTACK</v-btn>
        <v-btn text color="#FF4081" @click="heal">HEAL</v-btn>
        <v-btn text color="#F50057" @click="giveUp">GIVE UP</v-btn>
    </section>
    <section class="log" v-if="turns.length > 0">
            <ul>
                <li v-for="turn in turns"
                v-bind:key="turn.id"
                    :class="{'player-turn': turn.isPlayer, 'monster-turn': !turn.isPlayer}">
                    {{ turn.text }}
                </li>
            </ul>
    </section>
</div>
</div>
</template>


<script>
export default {
  name: 'App',
  components: {
  },
      data: function() {
        return {
        playerHealth: 100,
        monsterHealth: 100,
        gameIsRunning: false,
        turns: []
        }
    },
    methods: {
        startGame: function () {
            this.gameIsRunning = true;
            this.playerHealth = 100;
            this.monsterHealth = 100;
            this.turns = [];
        },
        attack: function () {
            var damage = this.calculateDamage(3, 10);
            this.monsterHealth -= damage;
            this.turns.unshift({
                isPlayer: true,
                text: 'Player hits Monster for ' + damage
            });
            if (this.checkWin()) {
                return;
            }

            this.monsterAttacks();
        },
        specialAttack: function () {
            var damage = this.calculateDamage(10, 20);
            this.monsterHealth -= damage;
            this.turns.unshift({
                isPlayer: true,
                text: 'Player hits Monster hard for ' + damage
            });
            if (this.checkWin()) {
                return;
            }
            this.monsterAttacks();
        },
        heal: function () {
            if (this.playerHealth <= 90) {
                this.playerHealth += 10;
            } else {
                this.playerHealth = 100;
            }
            this.turns.unshift({
                isPlayer: true,
                text: 'Player heals for 10'
            });
            this.monsterAttacks();
        },
        giveUp: function () {
            this.gameIsRunning = false;
        },
        monsterAttacks: function() {
            var damage = this.calculateDamage(5, 12);
            this.playerHealth -= damage;
            this.checkWin();
            this.turns.unshift({
                isPlayer: false,
                text: 'Monster hits Player for ' + damage
            });
        },
        calculateDamage: function(min, max) {
            return Math.max(Math.floor(Math.random() * max) + 1, min);
        },
        checkWin: function() {
            if (this.monsterHealth <= 0) {
                if (confirm('You won! New Game?')) {
                    this.startGame();
                } else {
                    this.gameIsRunning = false;
                }
                return true;
            } else if (this.playerHealth <= 0) {
                if (confirm('You lost! New Game?')) {
                    this.startGame();
                } else {
                    this.gameIsRunning = false;
                }
                return true;
            }
            return false;
        }
    },
};
</script>

<style>

#app {
    margin: 0 auto;
}
.text-center {
    text-align: center;
}

.appWidth {max-width: 800px; margin: 0 auto;}
.menu {
  color: #3F51B5;
  background-color: #C5CAE9;
  margin-bottom: 3rem;
}
.healthbar {
  height: 100%;
    background-color: #eee;
    margin: auto;
    transition: width 500ms;
    text-align: center;
    display: flex;
}

.controls, .log {
  display: flex;
justify-content: center;
    margin-top: 30px;
    text-align: center;
    border: 1px solid #ccc;
    box-shadow: 0px 3px 6px #ccc;

}
.log {padding: 10px 0 10px 0;}
.controls {padding: 0 10% 0 10%;}

.turn {
    margin-top: 20px;
    margin-bottom: 20px;
    font-weight: bold;
    font-size: 22px;
}

.log ul {
    list-style: none;
    font-weight: bold;
    text-transform: uppercase;
}

.log ul li {
    margin: 5px;
}

.log ul .player-turn {
    color: blue;
    background-color: #e4e8ff;
}

.log ul .monster-turn {
margin: 0 auto;
    color: red;
    background-color: #ffc0c1;
}
li.monster-turn {padding:5px;margin:0 auto;}
li.player-turn {padding:5px;margin:0 auto;}
button {
display: flex;
justify-content: center;
    font-size: 20px;
    background-color: #eee;
    padding: 12px;
    box-shadow: 0 1px 1px black;
    margin: 10px;
}

#start-game {
    background-color: #aaffb0;
}

#start-game:hover {
    background-color: #76ff7e;
}

#attack {
    background-color: #ff7367;
}

#attack:hover {
    background-color: #ff3f43;
}

#special-attack {
    background-color: #ffaf4f;
}

#special-attack:hover {
    background-color: #ff9a2b;
}

#heal {
    background-color: #aaffb0;
}

#heal:hover {
    background-color: #76ff7e;
}

#give-up {
    background-color: #ffffff;
}

#give-up:hover {
    background-color: #c7c7c7;
}
</style>
