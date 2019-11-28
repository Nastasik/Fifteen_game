<template>
  <div class="game-container">
    <div class="header">
      <h1>{{ msg }}</h1>
      <div class="count">
        <span>Ходов</span>
        <div>{{count}}</div>
      </div>
      <time class="time">
        <span>Время</span>
        <div>{{minutes}}:{{seconds}}</div>
      </time>
    </div>
    <div v-show="win === true" class="win">Победа</div>
    <ul @click.once = "timer" >
      <li v-for="(number, i) in numbers"
          @click = "move(i)"
          :class="{ hidden: number === 0, bright: number === i+1 }"
          :key="number">{{number}}
      </li>
    </ul>
  </div>
</template>

<script>
export default {
  name: 'Game',
  data () {
    return {
      msg: '15',
      count: 0,
      numbers: [...Array(16)].map((_, i) => i).sort(() => Math.random() - 0.5),   //Скорее иллюзия случайного перемешивания, но для игры достаточно
      seconds: '00',
      minutes: '00',
      win: false,
    }
  },

  methods: {
    move (i) {
        const p = (((i % 4) !== 0) ? i - 1 : 222);
        const n = ((((i - 3) % 4) !== 0) ? i + 1 : 222);
        const whereGoVar = [p, n, i - 4, i + 4].map(itemPos => {
          return { 
            value: this.numbers[itemPos], 
            pos: itemPos
          }
        });
        const rightWay = whereGoVar.filter(item => item.value === 0);
      
        if(rightWay.length !== 0) {
            const numbers = this.numbers.map(item => {
              if (item === 0) {
                return this.numbers[i];
              }
              if (item == this.numbers[i]) {
                return 0;
              }
              return item;
            })
            this.numbers = numbers;
            this.count= this.count + 1;
            this.win = this.numbers.every((item, i) => item === (i + 1) || item === 0);
        }
    },

    timer () {
        setInterval(() => {
          this.seconds = ((this.seconds < 9) ? "0" : null) + (Number(this.seconds) + 1);
          if (this.seconds === 60) {
            this.seconds = "00";
            this.minutes = ((this.minutes < 9) ? "0" : null) + (Number(this.minutes) + 1);
          }
        }, 1000);
    }
  }
}

</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style lang="scss" scoped>   
    @import '../assets/scss/main';
</style>
