<template>
  <div class="container">
    <h1>Calculator</h1>
    <div class="main-wrapper">
      <div class="calculator-wrapper">
        <div class="operator">
          {{currentoperator}}
        </div>
        <div class="display-wrapper">
          <div class="display">{{numberdisplay}}</div>
        </div>
        <div class="calculator-wrapper">
          <div class="cal-row">
            <button @click="calulation('7')" class="btn">
              7
            </button>
            <button @click="calulation('8')" class="btn">
              8
            </button>
            <button @click="calulation('9')" class="btn">
              9
            </button>
            <button @click="setCurrentOperator('x')" class="btn">
              x
            </button>
          </div>
          <div class="cal-row">
            <button @click="calulation('4')" class="btn">
              4
            </button>
            <button @click="calulation('5')" class="btn">
              5
            </button>
            <button @click="calulation('6')" class="btn">
              6
            </button>
            <button @click="setCurrentOperator('-')" class="btn">
              -
            </button>
          </div>
          <div class="cal-row">
            <button @click="calulation('1')" class="btn">
              1
            </button>
            <button @click="calulation('2')" class="btn">
              2
            </button>
            <button @click="calulation('3')" class="btn">
              3
            </button>
            <button @click="setCurrentOperator('+')" class="btn">
              +
            </button>
          </div>
          <div class="cal-row">
            <button @click="calulation('.')" class="btn">
              .
            </button>
            <button @click="setCurrentOperator('/')" class="btn">
              /
            </button>
            <button @click="calulation('0')" class="btn">
              0
            </button>
            <button @click="equels()" class="btn">
              =
            </button>
          </div>
          <div class="cal-row">
            <button @click="clear('clear')" class="btn-long">
              clear
            </button>
            <button @click="clear('clearhistory')" class="btn-long">
              clear history
            </button>
          </div>
        </div>
      </div>
      <div class="history-wrapper">
        <h2>History</h2>
        <div class="history"></div>
          <div class="history-items" v-for="(moment, index) in history" :key="index">{{moment.calulation}}
          <div class="result">Result {{moment.result}}</div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      history: [],
      numberdisplay: '',
      currentoperator: null,
      currentnumber: null,
      currentnumberarray: [],
      currentnumbersarray: [],
      operatorpressed: true,
      replace: null,
      result: ''
    }
  },
  methods: {
    calulation(numb) {
      if(this.result !== '') {
        this.result = '';
      }
      this.operatorpressed = false;
      this.currentnumberarray.push(numb);
      this.currentnumber = [...this.currentnumberarray].join().replace(/,/g, '');
      this.numberdisplay = this.currentnumber;
    },
    setResult() {
      switch (this.currentoperator) {
          case '+':
            this.result = parseFloat(this.currentnumbersarray[0]) + parseFloat(this.currentnumbersarray[1]);
            break;
          case '/':
            this.result = parseFloat(this.currentnumbersarray[0]) / parseFloat(this.currentnumbersarray[1]);
            break;
          case '-':
            this.result = parseFloat(this.currentnumbersarray[0]) - parseFloat(this.currentnumbersarray[1]);
            break;
          case 'x':
            this.result = parseFloat(this.currentnumbersarray[0]) * parseFloat(this.currentnumbersarray[1]);
            break;
        }
        this.reset();
    },
    reset() {
      this.makeHistory()
      this.currentnumbersarray = [];
      this.currentnumbersarray.push(this.result.toString());
      this.numberdisplay = this.result;
      this.operatorpressed = false;
    },
    makeHistory() {
      const historyobj = {
        calulation: this.currentnumbersarray[0].toString() + ' ' + this.currentoperator + ' ' + this.currentnumbersarray[1].toString(),
        result: this.result
      }
      this.history.push(historyobj);
    },
    equels() {
      if (!this.operatorpressed && this.currentoperator !== null) {
        this.currentnumberarray = [];
        this.currentnumbersarray.push(this.currentnumber);
        if(this.currentnumbersarray.length > 1) {
          this.setResult();
        }
        this.operatorpressed = true;
      }
    },
    setCurrentOperator(operator) {
      if (this.result !== '') {
        this.currentoperator = operator;
        this.result = '';
      }
      if (!this.operatorpressed) {
        this.currentnumberarray = [];
        this.currentnumbersarray.push(this.currentnumber);
        if(this.currentnumbersarray.length > 1) {
          this.setResult();
        }
        this.operatorpressed = true;
        this.currentoperator = operator;
      }
    },
    clear(clearType) {
      if (clearType === 'clear') {
        this.numberdisplay = '';
        this.currentnumberarray = [];
        this.currentnumbersarray = [];
        this.currentnumber = null;
        this.currentoperator = null;
      } else {
        this.history = [];
      }
    }
  }
}
</script>
 
<style>
.main-wrapper {
  display: flex;
}
.container {
  width: 100%;
  display: flex;
  flex-direction: column;
  align-items: center;
}
.main-wrapper {
  background: rgb(219, 215, 215);
  padding: 50px;
  border-radius: 10px;
}
.display-wrapper {
  display: flex;
  justify-content: center;
  margin: 20px 0;
}
.display {
  min-width: 150px;
  max-width: 500px;
  min-height: 30px;
  display: flex;
  justify-content: center;
  align-items: center;
  border: 1px solid red;
  border-radius: 5px;
}
.history-wrapper {
  margin-left: 50px;
}
.btn {
  width: 32px;
  height: 32px;
  margin: 4px;
  border: none;
  border-radius: 10px;
  transition: background 0.5s ease-out;
}
.btn-long {
  height: 32px;
  border: none;
  border-radius: 10px;
  margin: 4px;
  transition: background 0.5s ease-out;
}
.btn:hover, .btn-long:hover {
  background: rgb(219, 126, 126);
}
.calculator-wrapper {
  display: flex;
  flex-direction: column;
  align-items: stretch;
}
.history-items {
  margin: 10px 0;
}
.result {
  font-size: 1.8rem;
}
.operator {
  font-size: 1.8rem;
  width: 22px;
  height: 22px;
  border: 1px solid red;
  border-radius: 5px;
  padding: 2px;
  display: flex;
  justify-content: center;
  align-items: center;
  margin-left: 2px;
}
</style>