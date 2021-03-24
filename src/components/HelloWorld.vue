<template>
  <div class="container">
    <div class="btn-container">
      <div class="gacha-btn">
        <input type="button" id="single" value="1회 소환" v-on:click="gacha(1)">
      </div>
      <div class="gacha-btn">
        <input type="button" id="zyurengacha" value="10회 소환"  v-on:click="gacha(10)">
      </div>
      <div class="gacha-btn">
        <select v-model="selectedCard">
          <option v-for="list in itemList.ssrWpnList" v-bind:value="list" v-bind:key="list">
            {{list}}
          </option>
        </select>
        <input type="button" id="selectgacha" value="선택 소환" v-on:click="selectGacha(selectedCard)">
      </div>
      <Audio></Audio>
    </div>
    <Card :cardList="cardList" v-on:incCount="incTotalgrade"></Card>
    <div class="data-container">
      <p>가챠 횟수 : {{gCounter}} / SSR 웨폰 : {{ssrWCounter}} / SSR 아티 : {{ssrACounter}} / 지금까지 쓴 돈 : {{gCounter * 300}}</p>
      <input type="button" id="cReset" value="re"  v-on:click="gCounter = 0">
    </div>
  </div>
</template>

<script>
import Card from './card.vue'; //import Card from './card.vue'
import Audio from './audio.vue'; //import Audio from './audio.vue'

export default {
  name: 'HelloWorld',

  components: {
    Card,
    Audio,
  },

  data() {
    return {
      summon_times : 0,
      gCounter: 0,
      ssrWCounter: 0,
      ssrACounter: 0,
      gPercentage : [ 1, 2, 5, 14, 39, 39 ], // 등급별 가챠 퍼센티지 ( SSR웨폰 1 SSR아티 2 SR웨폰 5 SR아티 14 R웨폰/아티 각 39 )
                                             // 0 ~ 1 index 0
                                             // 1 ~ 3 index 1
                                             // 3 ~ 8 index 2
                                             // 8 ~ 22 index 3
                                             // 22 ~ 61 index 4
                                             // 61 ~ 100 index 5
      cardList: {
        gacha: [],
        grade: [],
      },
      selectedCard : '',
      gList: [],
      itemList: {
        ssrWpnList : [ '잉그리드', '사마엘', '테오도라', '실바노', '아이샤', '제르카', '츠바사'],
        ssrArtfList : [ '용이 된 영웅', '봄의 언덕', '요정의 초대', '태고의 바다', '만월을 등진 자', '광휘의 심판자' ],
        srWpnList : [ '프리티아', '아케나트', '피르모', '파르타니', '페이시', '데미안', '나츠네' ],
        srArtfList : [ '쏟아지는 불꽃', '창공의 소망', '그리도르의 등대', '잡히지 않는 달', '악령이 깃든 숲', '영광의 문' ],
        rWpnList : [ '시오도라', '헥토르', '로잘리아', '레비나', '라함', '샤킬라', '호타루'],
        rArtfList : [ '불의 성배', '바람의 곡옥', '봄의 노래', '얼음의 알', '악의의 형상', '천사의 날개' ],
      },
      tmpCard: {
        card: ['SSR', '잉그리드', 'Weapon'],
      }
    }
  },

  props: {
  },

  methods: {
    gacha: function(number) {
      var keys = Object.keys(this.itemList);
      this.cardList.gacha = [];
      this.cardList.grade = [];
      
      for(var i = 0;i<number;i++){
        var N = (Math.random() * 100);

        this.cardList.grade.splice(this.cardList.grade.length, 0, this.gradeSelector(N, 0, 0)); 
        this.cardList.gacha.splice(this.cardList.gacha.length, 0, this.cardSelector(keys[this.cardList.grade[i]]));
      }
    
      this.gCounter += number; // Total Cnt Increased

    },

    selectGacha: function(weapon) {
      do{
        this.gacha(10);
      } while(this.existselCard(weapon))
    },

    existselCard: function(weapon) {
      for(var i = 0;i<10;i++){
          if(this.cardList.gacha[i] == weapon){
            return false;
          }
      }

      return true;
    },
    
    gradeSelector: function(number, index, tmpSum) {
      if((number >= tmpSum) && (number < (tmpSum + this.gPercentage[index]))){
        return index;
      }
      else{
        tmpSum += this.gPercentage[index];
        return this.gradeSelector(number, ++index, tmpSum);
      }
    },

    cardSelector: function(key) {
      var N = (Math.random() * 100);
      var boundary = 100/(this.itemList[key].length);
      
      for(var i=0;i<this.itemList[key].length-1;i++){
        if((N >= (i*boundary)) && (N < ((i+1)*boundary))){
          break;
        }
      }

      return this.itemList[key][i];
    },

    incTotalgrade: function(grade) {
      if(grade == 0){
        this.ssrWCounter++;
      }
      else if(grade == 1){
        this.ssrACounter++;
      }
    },

  },
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
h3 {
  margin: 40px 0 0;
}
ul {
  list-style-type: none;
  padding: 0;
}
li {
  display: inline-block;
  margin: 0 10px;
}
a {
  color: #42b983;
}

p {
  margin: 0;
}

.container {
  max-width: 800px;
  display:flex;
  flex-direction: column;
  align-items: center;
  margin-left: auto;
  margin-right: auto;
}

.gacha-btn {
  margin: 3px;
}

.gacha-btn > input {
  width: 100px;
  height: 30px;
}

.gacha-btn > select {
  padding-top:2px;
  width: 100px;
  height: 30px;
}

#cReset {
  width: 30px;
  height: 20px;
  background: transparent;
  border: none;
  text-align: center;
  font-size: 12px;
  margin: 0px 10px;
}

.btn-container {
  display: inherit;
  justify-content: center;
}

.data-container {
  width: 100%;
  height: 20px;
  display: inherit;
  flex-direction: row;
  justify-content: center;
  border: solid 1px black;
}

.card-container {
  width: 800px;
  min-height: 468px;
  display: inherit;
  flex-direction: row;
  justify-content: center;
  align-content: center;
  flex-wrap: wrap;
}

.card {
    width: 140px;
    height: 224px;
    margin: 5px;
    border: none;
    text-align: center;
}
</style>