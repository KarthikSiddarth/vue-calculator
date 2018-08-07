<template>
  <div class="calculator">
    <div class="display">{{ current || 0}}</div>
    <div @click="clear" class="btn">C</div>
    <div @click="sign" class="btn">+/-</div>
    <div @click="percent" class="btn">%</div>
    <div @click="divide" class="btn">/</div>
    <div @click="append" class="btn">7</div>
    <div @click="append" class="btn">8</div>
    <div @click="append" class="btn">9</div>
    <div @click="multiply" class="btn">x</div>
    <div @click="append" class="btn">4</div>
    <div @click="append" class="btn">5</div>
    <div @click="append" class="btn">6</div>
    <div @click="subtract" class="btn">-</div>
    <div @click="append" class="btn">1</div>
    <div @click="append" class="btn">2</div>
    <div @click="append" class="btn">3</div>
    <div @click="add" class="btn">+</div>
    <div @click="append" class="btn zero">0</div>
    <div @click="dot" class="btn">.</div>
    <div @click="equal" class="btn">=</div>
  </div>
</template>

<script>
export default {
  data () {
    return {
      current: '',
      previous: '',
      operator: '',
      operatorClicked: false
    }
  },
  methods: {
    clear () {
      this.current = ''
    },
    sign () {
      this.current = this.current.charAt(0) === '-'
        ? this.current.slice(1)
        : `-${this.current}`
    },
    percent () {
      if (this.current === '') {
        this.current = 0
        return
      }
      this.current = `${parseFloat(this.current) / 100}`
    },
    append (e) {
      if (e === '.') {
        this.current = `${this.current}.`
        return
      }
      if (this.operatorClicked) {
        this.current = ''
        this.operatorClicked = false
      }
      this.current = `${this.current}${e.target.textContent}`
    },
    setPrevious () {
      this.previous = this.current
      this.operatorClicked = true
    },
    divide () {
      this.operator = (a, b) => a / b
      this.setPrevious()
    },
    multiply () {
      this.operator = (a, b) => a * b
      this.setPrevious()
    },
    subtract () {
      this.operator = (a, b) => a - b
      this.setPrevious()
    },
    add () {
      this.operator = (a, b) => a + b
      this.setPrevious()
    },
    dot () {
      if (this.current.indexOf('.') === -1) this.append('.')
    },
    equal () {
      this.current = `${this.operator(parseFloat(this.previous), parseFloat(this.current))}`
    }
  }
}
</script>

<style scoped>
  .calculator {
    display: grid;
    grid-template-columns: repeat(4, 1fr);
    grid-auto-rows: minmax(50px, auto);
    font-size: 40px;
  }

  .display {
    grid-column: 1/5;
    background-color: aqua;
  }

  .zero {
    grid-column: 1/3;
  }

  .btn {
    background-color: #eee;
    border: 1px solid #333;
  }
</style>
