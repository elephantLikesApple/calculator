<template>
  <div>
    <h1>Web Calculator</h1>
    <div class="calc-body">
      <h2>{{ panel }}</h2>
      <div class="buttons">
        <button @click="op" value="<<">&laquo;</button> <!-- << -->
        <button @click="op" value=">>">&raquo;</button> <!-- >> -->
        <button @click="op" value="%">&#37;</button> <!-- % -->
        <button @click="op" value="/">&#47;</button> <!-- / -->
        <button @click="back">&larr;</button><br> <!-- left arrow -->
        <button @click="num" value="7">7</button>
        <button @click="num" value="8">8</button>
        <button @click="num" value="9">9</button>
        <button @click="op" value="*">&#42;</button> <!-- * -->
        <button @click="unaryCalc" value="tan">tan</button><br>
        <button @click="num" value="4">4</button>
        <button @click="num" value="5">5</button>
        <button @click="num" value="6">6</button>
        <button @click="op" value="-">&#45;</button> <!-- - -->
        <button @click="unaryCalc" value="cos">cos</button><br>
        <button @click="num" value="1">1</button>
        <button @click="num" value="2">2</button>
        <button @click="num" value="3">3</button>
        <button @click="op" value="+">&#43;</button> <!-- + -->
        <button @click="unaryCalc" value="sin">sin</button><br>
        <button @click="del">CE</button>
        <button @click="num" value="0">0</button>
        <button @click="num" value=".">&#46;</button> <!-- . -->
        <button @click="binaryCalc('')">&equals;</button> <!-- = -->
        <button @click="op" value="^">x&#94;y</button> <!-- x^y -->
        
      </div>
    </div>
    <h4 v-html="this.displayExplain()"></h4>
  </div>
</template>

<script>

export default {
  name: 'Home',
  components: {},
  data() {
    return {
      panel : '0',
      numSize : 0,
      number : ['',''],
      operator : '',
      explain: '',
      opMap : new Map([
        ['+', '두 피연산지를 합하여 하나의 수로 만든다.'],
        ['-', '두 피연산지의 차를 구하여 하나의 수로 만든다.'],
        ['*', '두 피연산지를 곱하여 하나의 수를 만든다.\nX x Y일 때, X를 Y횟수만큼 더한다.'],
        ['/', '두 피연산지를 나누어 하나의 수를 만든다.\nX / Y일 때, X를 Y횟수만큼 뺀다.'],
        ['%', '두 피연산지를 나누어 나온 결과를 반환한다.\nX % Y일 때, X를 Y로 나누고 남은 나머지이다.'],
        ['^', 'x를 y횟수 만큼 곱한다. x의 y제곱'],
        ['<<', '첫 번째 피연산자를 이진수로 보고, 두번째 피연산자 만큼 왼쪽으로 자리를 올린다.\nX << Y일 때, X를 Y만큼 쉬프트한다.\n한 자리 쉬프트 할 때마다 2를 곱하는것과 같다.'],
        ['>>', '첫 번째 피연산자를 이진수로 보고, 두번째 피연산자 만큼 오른쪽으로 자리를 올린다.\nX >> Y일 때, X를 Y만큼 쉬프트한다.\n한 자리 쉬프트 할 때마다 2로 나누는것과 같다.'],
        ['tan', 'C가 직각인 삼각형 ABC에서, 각 A, B, C의 대변(마주보는 변)의 길이를 a,b,h라고 할 때,\ntan A = a/b'],
        ['cos', 'C가 직각인 삼각형 ABC에서, 각 A, B, C의 대변(마주보는 변)의 길이를 a,b,h라고 할 때,\ncos A = b/h'],
        ['sin', 'C가 직각인 삼각형 ABC에서, 각 A, B, C의 대변(마주보는 변)의 길이를 a,b,h라고 할 때,\nsin A = a/h'],
      ])

    }
  },
  methods: {
    displayExplain() {
      if(this.explain!=''){
        return this.explain.replace(/\n/g, '<br />');
      }
      return '';
    },
    op(evt) {
      this.explain = this.opMap.get(evt.target.value);
      let isDec = /\d+ ?$/;
      console.log(this.numSize);
      console.log(this.panel);
      if(this.numSize == 0) {
        // writeOperator()
        
        this.operator = evt.target.value;
        this.numSize+=1;
        this.panel = this.panel + ' ' + evt.target.value + ' ';
      }
      else if (this.numSize == 1) {
        if(isDec.test(this.panel)) {
          console.log('panel is dec');
          // 이전 입력이 숫자
          // 계산하기
          this.binaryCalc(evt.target.value);
        }
        else {
          // 이전 입력이 operator
          this.panel = this.panel.replace(this.operator, evt.target.value);
          this.operator = evt.target.value;
          console.log('numSize=1, panel is not dec')
          
        }
      }
      console.log('panel : '+this.panel+', numSize : '+this.numSize+', number : '+this.number+', operator : '+this.operator+', explain : '+this.explain)
    },
    num(evt) {
      let isDec = /\d+$/;

      if(this.panel == '0') {
        this.number[this.numSize] = evt.target.value;
        this.panel = evt.target.value;
      }
      else {
        this.number[this.numSize] += evt.target.value;
        this.panel = this.panel + evt.target.value;
      }
      this.explain = '';
      console.log('panel : '+this.panel+', numSize : '+this.numSize+', number : '+this.number+', operator :'+this.operator+', explain : '+this.explain)

    },
    back() {
      let isDec = /^-|[\d.]$/;
      if(isDec.test(this.panel)) {
        this.panel = this.panel.slice(0, -1)
        this.number[this.numSize] = this.number[this.numSize].slice(0, -1)
      }
      
      console.log('panel : '+this.panel+', numSize : '+this.numSize+', number : '+this.number+', operator :'+this.operator+', explain : '+this.explain)
    },
    del() {
      this.panel = '0';
      this.numSize = 0;
      this.number = [0, 0];
      this.operator = '';
      this.explain = '';
      console.log('panel : '+this.panel+', numSize : '+this.numSize+', number : '+this.number+', operator : '+this.operator+', explain : '+this.explain)
    },
    binaryCalc(nextOperator) {
      console.log('numSize : '+this.numSize);
      this.number[0] = parseFloat(this.number[0]);
      this.number[1] = parseFloat(this.number[1]);
      console.log(this.number);
      let res=0;
      switch(this.operator) {
        case '':
          res = this.panel;
          break;
        case '^':
          res = Math.pow(this.number[0] , this.number[1]);
          break;
        case '+':
          res = this.number[0] + this.number[1];
          break;
        case '-':
          res = this.number[0] - this.number[1];
          break;
        case '*':
          res = this.number[0] * this.number[1];
          break;
        case '/':
          res = this.number[0] / this.number[1];
          break;
        case '%':
          res = this.number[0] % this.number[1];
          break;
        case '<<':
          res = this.number[0] << this.number[1];
        case '>>':
          res = this.number[0] >> this.number[1];
          break;
      }
      console.log(res);
      debugger
      if(nextOperator == '') {
        this.panel = res+'';
        this.numSize = 0;
      }
      else {
        this.panel = res + ' '+nextOperator + ' ';
        this.numSize = 1;
      }
      this.number[0] = res;
      this.number[1] = '';
      this.operator = nextOperator;

      console.log('panel : '+this.panel+', numSize : '+this.numSize+', number : '+this.number+', operator : '+this.operator+', explain : '+this.explain)
    },
    unaryCalc(evt){

      if(this.operator != '') {
        this.binaryCalc();
      }
      debugger
      
      this.explain = this.opMap.get(evt.target.value);
      if (this.number[0] == '') this.number[0] = 0;
      this.number[0] = parseFloat(this.number[0]);
      this.operator = evt.target.value;
      let res = 0;
      switch(this.operator){
        case 'tan' :
          res = Math.tan((this.number[0] * 3.141592) / 180);
          break;
        case 'cos' :
          res = Math.cos((this.number[0] * 3.141592) / 180);
          break;
        case 'sin' :
          res = Math.sin((this.number[0] * 3.141592) / 180);
          break;
      }
      this.panel = res+'';
      this.number[0] = res;
      console.log('panel : '+this.panel+', numSize : '+this.numSize+', number : '+this.number+', operator : '+this.operator+', explain : '+this.explain)
      this.operator = '';
      this.numSize = 0;
    }
  },
}
</script>

<style scoped>
.calc-body {
  margin: 2px auto;
  padding: 10px;
  background-color: #999999;
  width: 350px;
}

h2 {
  margin: 2px auto;
  padding: 5px 10px;
  width: 90%;
  min-height: 27.2px;
  text-align: right;
  background-color: #e6e6e6;
  overflow: hidden;
}

.buttons {
  margin: auto;
  padding: 10px;
}

.buttons button {
  font-size: 1em;
  font-weight: bold;
  width: 50px;
  height: 50px;
  margin: 3px;
}

.hide {
  visibility: hidden;
}
</style>