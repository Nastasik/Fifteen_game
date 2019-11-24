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
    <ul v-on:click.once = "timer" >
      <li v-for="(number, i) in numbers"
          v-on:click = "move"
          :data-i = "i"
          v-bind:class="{ hidden: number === 0, bright: number === i+1 }"
          v-bind:key="number">{{number}}
      </li>
    </ul>
  </div>
</template>

<script>
export default {
  name: 'HelloWorld',
  data () {
    return {
      msg: '15',
      count: 0,
      numbers: [...Array(16)].map((_, i) => i).sort(() => Math.random() - 0.5),   //Скорее иллюзия случайного перемешивания, но для игры достаточно
      container: document.querySelector('ul'),
      seconds: '00',
      minutes: '00',
      win: false,
    }
  },

  methods: {
    move (e) {
        const i = Number(e.target.dataset.i);
        const p = (((i % 4) !== 0) ? i - 1 : 222);
        const n = ((((i - 3) % 4) !== 0) ? i + 1 : 111);
        const whereGoVar = [p, n, i - 4, i + 4].map(itemPos => {
          return { 
            value: this.numbers[itemPos], 
            pos: itemPos
          }
        });
        const rightWay = whereGoVar.filter(item => item.value === 0);
      
        if(rightWay.length !== 0) {
            this.numbers = this.numbers.map(item => {
              if (item === 0) {
                return Number(e.target.innerHTML);
              }
              if (item == e.target.innerHTML) {
                return 0;
              }
              return item;
            })
            this.count= this.count + 1;
            this.win = this.numbers.forEach((item, i) => item === (i + 1) || item === 0) ? true : false;
        }
    },

    timer () {
        setInterval(() => {
          this.seconds = ((this.seconds < 10) ? "0" : null) + (Number(this.seconds) + 1);
          if (this.seconds === 60) {
            this.seconds = "00";
            this.minutes = ((this.minutes < 10) ? "0" : null) + (Number(this.minutes) + 1);
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
