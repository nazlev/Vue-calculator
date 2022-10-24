<template>
  <div id="app" >
    <input type="number" :class="{active: operand.current === 1}" v-model="operand1"/>
    <input type="number" :class="{active: operand.current === 2}" v-model="operand2"/>
    <p>Результат {{ result }}</p>

    <div v-if="error" class="error">{{ error }}</div>

    <button v-for="item, index of operations" :key="index" v-on:click="currentOperation = item">{{ item }}</button>

    <div class="keyboards">
    <label><input type="checkbox" v-on:click="keyboard" :checked="isKeyBoardShow">Показать клавиатуру</label>

    <div v-if="isKeyBoardShow">
      <button v-for="num of [0,1,2,3,4,5,6,7,8,9]" :key="num" v-on:click="keyDown(num)">{{ num }}</button>
      <button v-on:click="backspace">-</button>
      <br>
      <input type="radio" id="one" :value="{current: 1}" v-model="operand"/>
      <label for="one">Один</label>
      <input type="radio" id="two" :value="{current: 2}" v-model="operand"/>
      <label for="two">Два</label>
    </div>
  </div>
  </div>
</template>

<script>
export default {
  name: 'Calculator',
  data() {
    return {
      operand: 0,
      operand1: 0,
      operand2: 0,
      currentOperation: '+',
      error: '',
      operations: [
        '+',
        '-',
        '*',
        '/',
        '**',
        '%'
      ],
      logList: [],
      isKeyBoardShow: false
    }
  },
  computed: {
    result() {
      return this.doCalc(this.currentOperation);
    }
  },


  methods: {
    backspace() {
      if(this.operand.current === 1) {
        this.operand1 = this.operand1.slice(0, -1);
        this.operand1 = this.operand1 === 0 ? '' : this.operand1
      } else if(this.operand.current === 2) {
        this.operand2 = this.operand2.slice(0, -1);
        this.operand2 = this.operand2 === 0 ? '' : this.operand2
      }
    },
    keyDown(num) {
      if(this.operand.current === 1) {
        this.operand1 = this.operand1 === 0 ? '' : this.operand1;
        this.operand1 = String(this.operand1) + String(num)
      } else if(this.operand.current === 2) {
        this.operand2 = this.operand2 === 0 ? '' : this.operand2;
        this.operand2 = String(this.operand2) + String(num)
      }
    },
    keyboard() {
      this.isKeyBoardShow = !this.isKeyBoardShow
    },
    checkOperand(numberOperand) {
      this.currentEnter = numberOperand
    },
    sum() {
      return Number(this.operand1) + Number(this.operand2)
    },
    minus() {
      return this.operand1 - this.operand2
    },
    multy() {
      return this.operand1 * this.operand2
    },
    div() {
      if(+this.operand2 === 0) {
        this.error = 'На ноль делить нельзя';
        return
      }
      return this.operand1 / this.operand2
    },
    deg() {
      return this.operand1 ** this.operand2
    },
    int() {
      if(+this.operand2 === 0) {
        this.error = 'На ноль делить нельзя';
        return
      }
      return Math.trunc(this.operand1 / this.operand2)
    },
    doCalc(operand) {
      this.logList.push(`${this.operand1} ${operand} ${this.operand2}`)
      this.error = '';
      switch(operand) {
        case '+': 
          return this.sum();
          break;
        case '-': 
          return this.minus();
          break;
        case '*': 
          return this.multy();
          break;
        case '/': 
          return this.div();
          break;
        case '**': 
          return this.deg();
          break;
        case '%': 
          return this.int();
          break;
      }
    }
  }
}
</script>

<style lang="scss">
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}

.error {
  color: darkred;
  border: 1px solid darkred;
  display: block;
  padding: 10px 20px;
  margin: 20px 0;
}

.active {
  border: 1px solid blue
}

.keyboards {
  margin-top: 10px;
  display: flex;
  flex-direction: column;
  align-items: center;
}
</style>
