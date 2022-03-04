<template>
  <p>Monster Game
</p>
<table>
<tr><td>Spiel A </td> <td>Monster</td></tr>
<tr><td></td><td></td></tr>
<tr><td>
<div class="progress-bar-title">
<p style="color: black">% {{ progressbarValue1 }}</p>
</div>
<div class="progressbar">
<div class="progressbar2" :style="{width:progressbarValue1 + '%'}"></div>
</div>
</td>
<td>
<div class="progress-bar-title">
<p style="color: black">% {{ progressbarValue2 }}</p>
</div>
<div class="progressbar">
<div class="progressbar2" :style="{width:progressbarValue2 + '%'}"></div>
</div>
</td>
</tr>
<tr><td></td></tr>
<tr>
<td><button @click="attack()">ATTACK </button> </td>
<td><button @click="specialattack()" >SPECIAL ATTACK </button></td>
<td><button @click="firstAid()">FIRST AID </button></td>
<td><button @click="giveUp()">GIVE UP! </button></td>

</tr>
<tr><td> You reduced the monster's health:({{ progressbarValue2Math }})</td></tr>
<tr><td> monster attack:({{ progressbarValue1Math }})</td></tr>

</table>
</template>

<script>
export default {
  name: 'monster-game',
  data(){
    return {
      progressbarValue1:100,
      progressbarValue2:100,
      progressbarValue1Math:0,
      progressbarValue2Math:0,
      firstAidValue:0,
      gewinnSituation:false,

    }
  }, 
  methods: {
    attack(){
      const gewinnen = this.gewinn();
    if ( gewinnen === false){
      this.progressbarValue1Math = Math.floor(Math.random() * 10);
      this.progressbarValue2Math = Math.floor(Math.random() * 10);

      this.progressbarValue1 -= this.progressbarValue1Math;
      this.progressbarValue2 -= this.progressbarValue2Math;
      if (this.progressbarValue1 < 0 ){
        this.progressbarValue1 = 0;
      }
          if (this.progressbarValue2 < 0 ){
        this.progressbarValue2 = 0;
      }

}
    },
      firstAid(){
  
        this.firstAidValue = Math.floor(Math.random() * 10);
        this.progressbarValue2Math = Math.floor(Math.random() * 10);

      this.progressbarValue1 += this.firstAidValue;
      if (this.progressbarValue1 >100){
        this.progressbarValue1 = 100;
      }
      this.progressbarValue2 -= this.progressbarValue2Math;
},   specialattack(){
    const gewinnen = this.gewinn();
    if ( gewinnen === false){
      
        this.progressbarValue1Math = Math.floor(Math.random() * 10);
        this.progressbarValue2Math = Math.floor(Math.random() * 10);
      this.progressbarValue1 -= this.progressbarValue1Math;
      this.progressbarValue2 -= this.progressbarValue2Math;
      
}
    },
    gewinn(){
      if ( this.progressbarValue1 == 0 && this.progressbarValue2 == 0){
        alert("lost both of you");
      } else if ( this.progressbarValue1 <= 0 ) {
      alert("Monster gewinnen");
      this.gewinnSituation= true;
      }else   if ( this.progressbarValue2 <= 0) {
      alert("You gewinnen");
            this.gewinnSituation= true;
      }
      return this.gewinnSituation;
    },
        giveUp(){
      alert("You gave up");
      this.progressbarValue1=100;
      this.progressbarValue2=100;

 
    },
  }
}    
  

</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
.progressbar {
  width:250px;background-color:#00a0b0;border-radius:6px;padding:3px;
}
.progressbar2 {
  background-color: #ffffff;
  height:10px;border-radius:4px;
}
</style>
