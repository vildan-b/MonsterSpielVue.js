<template>
<div>
<div id="app">
    <section class="row">
        <div class="small-6 columns">
            <h1 class="text-center">Player</h1>
            <div class="healthbar">
                <div :style=" playerProgress" class="healthbar text-center" style="background-color: green; margin: 0; color: white;">
{{ player_heal}}%
                </div>
            </div>
        </div>
        <div class="small-6 columns">
            <h1 class="text-center">Monster</h1>
            <div class="healthbar">
                <div :style="monsterProgress" class="healthbar text-center" style="background-color: green; margin: 0; color: white;">
{{ monster_heal}}%
                </div>
            </div>
        </div>
    </section>
    
    <section class="row controls" v-if="!game_is_on">
        <div class="small-12 columns">
            <button id="start-game" @click="startGame()">NEW PLAY</button>
        </div>
    </section>
    <section class="row controls" v-if="game_is_on">
        <div class="small-12 columns">
            <button id="attack" @click="attack()">ATTACK</button>
            <button id="special-attack" @click="specialAttack()">SPECIAL ATTACK</button>
            <button id="heal" @click="heal_up()" >FIRST AID</button>
            <button id="give-up" @click="giveUp()">GIVE UP!</button>
        </div>
    </section>
    <section class="row log" v-if="logs.length > 0">
        <div class="small-12 columns">
            <ul>
                <li 
                    :class="{ 'player-turn' : log.turn == 'p', 'monster-turn' : log.turn == 'm' }"
                    v-for="log in logs" :key="log"> {{ log.text }}

                </li>
            </ul>
        </div>
    </section>
</div>
</div>
</template>

<script>
// eslint-disable-next-line
/* eslint-disable */
export default {
  name: 'monsterBeraber',
  methods: {
      startGame(){
          this.game_is_on = true;
      },
      attack(){
          var point = Math.ceil(Math.random() * this.attack_multiple);
    this.monster_heal -= point;
    this.monsterattack();
   this.add_to_log( { turn : "p", text :  this.log_text.attack+ point});
      }, 

      monsterattack(){
          var point= Math.ceil(Math.random() * this.monster_attack_multiple);
             this.player_heal -= point;
           this.add_to_log( { turn : "m", text : this.log_text.monster_attackt +  point});

      }, 
      specialAttack(){
           var point = Math.ceil(Math.random() * this.special_attack_multiple);
            this.monster_heal -= point;
                        this.monsterattack();

           this.add_to_log( { turn : "p", text : this.log_text.attack + point});

      },
      heal_up(){
            var point = Math.ceil(Math.random() * this.heal_up_multipe);
    this.player_heal += point;
           this.add_to_log( { turn : "p", text : this.log_text.heal_up +  point});

    this.monsterattack();
     },
      giveUp(){
          this.player_heal =0;
                 this.add_to_log( { turn : "p", text : this.log_text.give_up});

      },
      add_to_log(log){
          this.logs.push(log);

      }

  },
  watch:{
      player_heal(value){
          if(value <=0){
             this.player_heal = 0; 
            if( confirm("You lost the game. Do you want to try again?")){
               this.player_heal = 100;
               this.monster_heal = 100; 
               this.logs = [];
            }
          } else if( value >= 100){
              this.player_heal =100;
          }
      },
      monster_heal(value){
          if(value <=0){
             this.monster_heal = 0; 
              if( confirm("You won the game... Do you want to try again?")){
               this.player_heal = 100;
               this.monster_heal = 100; 
            }
          }
      }
  },
  computed: {
      playerProgress(){
          return {
              width: this.player_heal + '%'
          }
      },
      monsterProgress(){
          return {
              width: this.monster_heal + '%'
          }
      }

  },

  data() {
    return {
        player_heal:100,
        monster_heal:100,
        game_is_on: false,
        attack_multiple:10,
        special_attack_multiple:25,
        monster_attack_multiple:15,
        heal_up_multipe:20,
        log_text: {
            attack : "player move ",
            special_attack: "special player move ",
            monster_attackt: "monster move ",
            heal_up:"First aid ",
            give_up: "Player give up "
        },
        logs : []
    }
  }

}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
.text-center {
    text-align: center;
}

.healthbar {
    width: 100%;
    height: 40px;
    background-color: #eee;
    margin: auto;
    transition: width 500ms;
}

.controls, .log {
    margin-top: 30px;
    text-align: center;
    padding: 10px;
    border: 1px solid #ccc;
    box-shadow: 0px 3px 6px #ccc;
}

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
    color: red;
    background-color: #ffc0c1;
}

button {
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