<template>
  <view class="container">
    <nb-text class="title">TIC-TAC-TOE</nb-text>
    <view class="reset">
      <text>{{winner}}</text>
      <nb-button 
        :on-press="reset" 
        bordered success
        v-if="finished" 
        class='resetBtn'
      >
        <nb-text style="color: #fff">Reset</nb-text>
      </nb-button>
    </view>
    <view class="board" :class="{disable: finished}">
      <nb-button 
        bordered info
        class='field'
        v-for="(n, index) in boards" 
        :key="index"
        :on-press="() => check(index)"
      >
        <nb-text class='point'>{{ n.text }}</nb-text>
      </nb-button>
    </view>
  </view>
</template>

<script>
import Vue from "vue-native-core";
import { VueNativeBase } from "native-base";

// registering all native-base components to the global scope of the Vue
Vue.use(VueNativeBase);

import * as Font from 'expo-font';
import { Ionicons } from '@expo/vector-icons';

export default {
  data: () => ({
    lines: [
      [0, 1, 2],
      [3, 4, 5],
      [6, 7, 8],
      [0, 3, 6],
      [1, 4, 7],
      [2, 5, 8],
      [0, 4, 8],
      [2, 4, 6]
    ],
    boards: [
      {text: ''},
      {text: ''},
      {text: ''},
      {text: ''},
      {text: ''},
      {text: ''},
      {text: ''},
      {text: ''},
      {text: ''},
    ],
    xPlayer: [],
    oPlayer: [],
    currentPlayer: 'X',
    finished: false,
    winner: ''
  }),
  methods: {
    check(n){
      if(this.boards[n].text || this.finished) return;
      
      this.boards[n].text = this.currentPlayer;

      if(this.currentPlayer == 'X'){
        this.currentPlayer = 'O';
        this.xPlayer.push(n);
        this.checkIfSomeoneWin('xPlayer')
      } else {
        this.currentPlayer = 'X';
        this.oPlayer.push(n);
        this.checkIfSomeoneWin('oPlayer')
      }
    },
    checkIfSomeoneWin(player){
      let gamer = this[player];
      if(gamer.length > 2){
        this.lines.forEach(line => {
          let success = 0;
          gamer.forEach(checked => {
            if(line.some(e => e == checked)){
              success++;
              if(success == 3){
                this.winner = 'Win ' + player;
                this.finished = true;
              }
            }
          })
        })
      }

      if(!this.finished){
        this.checkIfDrow();
      }
    },
    reset(){
      this.xPlayer = [];
      this.oPlayer = [];
      this.boards.forEach(e => e.text = '');
      this.winner = '';
      this.currentPlayer = 'X'
      this.finished = false;
    },
    checkIfDrow(){
      if(this.xPlayer.length + this.oPlayer.length == 9){
        this.winner = 'Drow';
        this.finished = true;
      }
    }
  },
  async created(){
    await Font.loadAsync({
      Roboto: require('native-base/Fonts/Roboto.ttf'),
      Roboto_medium: require('native-base/Fonts/Roboto_medium.ttf'),
      ...Ionicons.font,
    });
  },
}
</script>

<style>
.container {
  background-color: white;
  align-items: center;
  justify-content: center;
  flex: 1;
}
.title {
  color: blue;
  font-size: 35px;
}
* {
  box-sizing: border-box;
  margin: 0;
  padding: 0;
}
.reset {
  min-height: 45px;
  margin-bottom: 25px;
}
.resetBtn {
  background-color: green;
  padding: 8px 12px;
  border-radius: 6px;
  color: #fff;
}
.board {
  width: 450px;
  height: 450px;
  display: flex;
  flex-direction: row;
  flex-wrap: wrap;
  margin: auto;
}
.field {
  width: 150;
  height: 150;
  display: flex;
  justify-content: center;
  align-items: center;
}
.point {
  font-size: 70px;
}
</style>
