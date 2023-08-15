<template>
  <div class="bg-gray-200 w-full min-h-screen pt-10">
    <div class="flex flex-col w-2/3 laptop:w-1/3 bg-white justify-center m-auto rounded shadow-xl">
      <h1 class="text-center mt-5 text-slate-600 text-lg tablet:text-2xl mb-10 font-bold">Калькулятор онлайн</h1>

      <div class="border rounded p-5 m-auto flex flex-col w-5/6 mb-5">
        <input class="text-right text-2xl font-medium mr-1" v-model="inputValue" @keydown.enter="calculateResult">
        <hr>
        <input class="text-right text-sm opacity-75 font-medium mr-1" v-model="expression" disabled>
      </div>

      <div class="flex justify-between flex-wrap mb-5">
        <button class="text-orange-500 w-1/4 my-2" type="button" @click="inputValue = '0'">C</button>
        <button class="text-orange-500 w-1/4 my-2" type="button" @click="deleteSymbol">X</button>
        <button class="text-orange-500 w-1/4 my-2" type="button" @click="saveVal">M</button>
        <button class="text-orange-500 w-1/4 my-2" type="button" @click="addExpression('/')">/</button>
        <button class="w-1/4 my-2" type="button" @click="putSymbol('7')">7</button>
        <button class="w-1/4 my-2" type="button" @click="putSymbol('8')">8</button>
        <button class="w-1/4 my-2" type="button" @click="putSymbol('9')">9</button>
        <button class="text-orange-500 w-1/4 my-2" type="button" @click="addExpression('*')">*</button>
        <button class="w-1/4 my-2" type="button" @click="putSymbol('4')">4</button>
        <button class="w-1/4 my-2" type="button" @click="putSymbol('5')">5</button>
        <button class="w-1/4 my-2" type="button" @click="putSymbol('6')">6</button>
        <button class="text-orange-500 w-1/4 my-2" type="button" @click="addExpression('-')">-</button>
        <button class="w-1/4 my-2" type="button" @click="putSymbol('1')">1</button>
        <button class="w-1/4 my-2" type="button" @click="putSymbol('2')">2</button>
        <button class="w-1/4 my-2" type="button" @click="putSymbol('3')">3</button>
        <button class="w-1/4 my-2" type="button" @click="addExpression('+')">+</button>
        <button class="w-1/4 my-2" type="button" @click="delExpression">DEL</button>
        <button class="text-orange-500 w-1/4 my-2" type="button" @click="putSymbol('0')">0</button>
        <button class="w-1/4 my-2" type="button" @click="putSymbol('.')">.</button>
        <button class="text-orange-500 w-1/4 my-2" type="button" @click="calculateResult">=</button>
      </div>
    </div>

    <div class="flex flex-col mt-5 ml-6 small:ml-24">
      <h2 class="text-1xl font-medium mb-3">Обозначения</h2>
      <div class="flex flex-col justify-center">
        <div class="mb-3">
          <span>Знак - C</span>
          <p>Стереть все цифры</p>
        </div>
        <div class="mb-3">
          <span>Знак - X</span>
          <p>Стереть последнюю цифру</p>
        </div>
        <div class="mb-3">
          <span>Знак - M</span>
          <p>Сохранить значение / Вставить сохраненное значение</p>
        </div>
        <div class="mb-3">
          <span>Знак - DEL</span>
          <p>Удалить нижнее выражение</p>
        </div>
      </div>
    </div>

  </div>
</template>

<script>
export default {
  name: 'App',
  data () {
    return {
      inputValue: '0',
      expression: '',
      savedVal: ''
    }
  },
  mounted () {
    document.addEventListener('keydown', this.clickTrack)
  },
  beforeUnmount () {
    document.removeEventListener('keydown', this.clickTrack)
  },
  EXPRESSION_LIST: ['-', '+', '/', '*'],
  NUMBERS: ['1', '2', '3', '4', '5', '6', '7', '8', '9'],
  methods: {
    putSymbol (num) {
      this.inputValue === '0' ? this.inputValue = num : this.inputValue += num
    },
    deleteSymbol () {
      const inputLength = this.inputValue.length
      inputLength > 1 ? this.inputValue = this.inputValue.slice(0, this.inputValue.length - 1) : this.inputValue = '0'
    },
    addExpression (symbol) {
      this.expression += this.inputValue + symbol
      this.inputValue = '0'
    },
    calculateResult () {
      this.expression += this.inputValue
      // eslint-disable-next-line no-eval
      this.inputValue = eval(this.expression)
      this.expression = ''
    },
    delExpression () {
      this.expression = this.expression.slice(0, this.expression.length - 1)
    },
    saveVal () {
      if (this.savedVal) {
        this.inputValue = this.savedVal
        this.savedVal = ''
      } else {
        this.savedVal = this.inputValue
        this.inputValue = '0'
      }
    },
    clickTrack (e) {
      console.log(e)
      if (e.code === 'Enter') {
        this.calculateResult()
      }
      if (this.$options.NUMBERS.includes(e.key) || this.$options.EXPRESSION_LIST.includes(e.key)) {
        this.putSymbol(e.key)
      }
      if (e.key === 'm') {
        this.saveVal()
      }
    }
  }
}
</script>

<style>
  html {
    font-size: 20px;
  }
</style>
