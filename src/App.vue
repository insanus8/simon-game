<template>
  <div id="app">
    <h1>Игра Саймон</h1>
    <h2 v-show="losing" class="losing">Игра окончена.</h2>
    <div class="contaner">
      <ul class="simon">
        <li class="simon__wrapp" v-for="(btn, index) in btns" :key="index">
          <button class="simon__btn"
          :class="btn.state ? 'active': ''"
          :style="{backgroundColor: btn.color}"
          @click="pressingButton(btn.color, index)"></button>
        </li>
      </ul>
      <div class="info-game">
        <div class="info-game__menu">
          <h2>
            Раунд {{roundNumber}}
          </h2>
          <button class="btn" @click="startRound()">
            Начать игру
          </button>
        </div>
        <levels :class="gameContinues ? 'disable' : ''" ref="time"></levels>
      </div>
    </div>
  </div>
</template>

<script>
import levels from './components/levels'

export default {
  name: 'App',
  components:{
    levels
  },
  data() {
    return {
      roundNumber: 0,
      btns: [
        {
          color: 'red',
          musick: 'audio/1.ogg',
          state: false,
        },
        {
          color: 'blue',
          musick: 'audio/2.ogg',
          state: false,
        },
        {
          color: 'yellow',
          musick: 'audio/3.ogg',
          state: false,
        },
        {
          color: 'green',
          musick: 'audio/4.ogg',
          state: false,
        }
      ],
      sequence: [],
      gameContinues: false,
      losing: false,
      sequenceLength: 0,
      clickNumber: 0,
    }
  },
  methods: {
    changeSequence: function() {
      let number = Math.floor(Math.random() * 4)
      this.sequence.push(this.btns[number])
      this.sequenceLength =  this.sequence.length
      this.clickNumber = 0
      this.losing = false
    },
    startRound: function() {
      this.changeSequence()  
      let time = this.$refs.time.$data.time;
          
      for (const key in this.sequence) {
        setTimeout(()=>{
          this.sequence[key].state = true
          new Audio(this.sequence[key].musick).play()
          setTimeout(()=>{
            this.sequence[key].state = false            
          }, time - 200)
        }, time*key)
      }
      setTimeout(()=> this.gameContinues = true, time*this.sequenceLength)
    },
    pressingButton: function(color, index) {
      if (this.gameContinues) {
        new Audio(this.btns[index].musick).play()
        if (this.sequence[this.clickNumber].color === color) {
          this.clickNumber++
          if (this.clickNumber == this.sequenceLength) {
            setTimeout(()=>{
              this.startRound()
            }, this.$refs.time.$data.time)
            this.roundNumber++
            return
          }
        } else {
          this.sequence = []
          this.gameContinues = false
          this.roundNumber = 0
          this.losing = true
          return
        }
      }
    },
    
  }
}
</script>

<style>
* {
  -webkit-box-sizing: border-box;
          box-sizing: border-box;
}
body {
  font-family: Arial, serif;
  font-size: 16px;
  width: 100%;
  height: 100vh;
  padding: 0;
  margin: 0;
}

h1 {
  font-size: 2em;
  margin: 0;
  margin-bottom: 50px
}

h2 {
  font-size: 1.4em;
  margin: 0;
  margin-bottom: 10px
}

h3 {
  font-size: 1em;
  margin: 0;
  margin-bottom: 10px
}

button {
  border: none;
  padding: 0;
  margin: 0;
  cursor: pointer;
}

ul {
  list-style-type: none;
  padding: 0;
  margin: 0;
}

.btn {
  padding: 15px 30px;
  font-family: inherit;
  font-size: 1em;
  font-weight: 700;
  color: white;
  background-color: blue;
  -webkit-transition: opacity .4s;
  -o-transition: opacity .4s;
  transition: opacity .4s;
}

.btn:hover {
  opacity: .6;
}

.btn:active {
  opacity: .4;
}

label, input[type="radio"] {
  cursor: pointer;
}

#app {
  display: -webkit-box;
  display: -ms-flexbox;
  display: flex;
  -webkit-box-orient: vertical;
  -webkit-box-direction: normal;
      -ms-flex-direction: column;
          flex-direction: column;
  -webkit-box-align: center;
      -ms-flex-align: center;
          align-items: center;
  padding: 100px 200px;
  width: 100%;
  height: 100%;
}

.contaner {
  display: -webkit-box;
  display: -ms-flexbox;
  display: flex;
  -webkit-box-align: center;
      -ms-flex-align: center;
          align-items: center
}

.losing {
  color: red;
  margin: 10px;
}

.simon {
  -ms-flex-negative: 0;
      flex-shrink: 0;
  display: -webkit-box;
  display: -ms-flexbox;
  display: flex;
  -ms-flex-wrap: wrap;
      flex-wrap: wrap;
  width: 400px;
  height: 400px;
  background-color: black;
  overflow: hidden;
  margin-right: 50px
}

.simon__wrapp {
  -webkit-box-flex: 1;
      -ms-flex: 1 0 50%;
          flex: 1 0 50%;
}

.simon__btn {
  width: 100%;
  height: 100%;
  opacity: .4;
  -webkit-transition: opacity .4s, border .6s;
  -o-transition: opacity .4s, border .6s;
  transition: opacity .4s, border .6s;
}

.simon__btn.active,
.simon__btn:active {
  opacity: 1;
}

.disable {
  opacity: .5;
  pointer-events: none
}

@media screen and (max-width: 1010px) {
  #app {
    padding: 100px 50px;
  }
  .simon {
    width: 200px;
    height: 200px;
  }
}

@media screen and (max-width: 520px) {
  #app {
    padding: 30px;
    text-align: center;
  }
  .simon {
    margin-right: 0;
    margin-bottom: 30px;
  }
  .contaner {
    -webkit-box-direction: normal;
        -ms-flex-direction: column;
            flex-direction: column;
  }
}
</style>