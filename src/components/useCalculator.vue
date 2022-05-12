<template>
  <div class="main">
    <div class="calculator">
      <div class="calc">
        <small v-if="selectedOperation"
          >{{ prevNum }} {{ selectedOperation }} {{ currentNum }}</small
        >
      </div>
      <div>
        <h1 class="value">{{ currentNum }}</h1>
      </div>
      <div class="buttons">
        <button class="btn clear" @click="clear">AC</button>
        <button class="btn delete" @click="deleteValue">DEL</button>
        <button class="btn" @click="pressed('1')">1</button>
        <button class="btn" @click="pressed('2')">2</button>

        <button class="btn" @click="pressed('3')">3</button>
        <button class="btn" @click="pressed('4')">4</button>
        <button class="btn" @click="pressed('5')">5</button>
        <button class="btn" @click="pressed('6')">6</button>

        <button class="btn" @click="pressed('7')">7</button>
        <button class="btn" @click="pressed('8')">8</button>
        <button class="btn" @click="pressed('9')">9</button>
        <button class="btn" @click="pressed('0')">0</button>

        <button class="btn" @click="pressed('*')">*</button>
        <button class="btn" @click="pressed('/')">/</button>
        <button class="btn" @click="pressed('.')">.</button>
        <button class="btn" @click="pressed('+')">+</button>

        <button class="btn" @click="pressed('-')">-</button>
        <button class="btn" @click="pressed('=')">=</button>
      </div>
    </div>
  </div>
</template>

<script>
import { ref, onMounted} from 'vue'

export default {
  setup() {
    const operations = ['+', '-', '*', '/']
    const numbers = ['1', '2', '3', '4', '5', '6', '7', '8', '9', '0', '.']
    const currentNum = ref('')
    const prevNum = ref('')
    const selectedOperation = ref('')

console.log(currentNum.value.substring(1,2))

    const pressed = value => {
      if (value === '=' || value === 'Enter') result()
      else if (currentNum.value.length > 9) alert('KEY ERROR: Display limit reached')
      else if (currentNum.value.substring(0, 1) === '.') replaceDot(value)
      else if (value === '.' ) dot()
      else if (operations.includes(value)) applyOperation(value)
      else if (numbers.includes(value)) appendNumber(value)
    }

    const result = () => {
      if (selectedOperation.value === '/' && currentNum.value === '0') {
        clear()
      }
      calculate()
      
    }

    const dot = () => {
      if (currentNum.value.indexOf('.') === -1) currentNum.value += '.'
    }

    const replaceDot = (value) => {
           currentNum.value = value
    }

    const appendNumber = value => {
      currentNum.value = currentNum.value + value
    }

    const applyOperation = value => {
      calculate()
      prevNum.value = currentNum.value
      currentNum.value = ''
      selectedOperation.value = value
    }

    const calculate = () => {
      if (selectedOperation.value === '*') multiply()
      else if (selectedOperation.value === '/') divide()
      else if (selectedOperation.value === '-') subtract()
      else if (selectedOperation.value === '+') sum()
      prevNum.value = ''
      selectedOperation.value = ''
    }

    const multiply = () => {
      currentNum.value = prevNum.value * currentNum.value
    }

    const divide = () => {
      currentNum.value = prevNum.value / currentNum.value
    }

    const subtract = () => {
      currentNum.value = prevNum.value - currentNum.value
    }

    const sum = () => {
      currentNum.value = +prevNum.value + +currentNum.value
    }

    const clear = () => {
      currentNum.value = ''
      prevNum.value = ''
      selectedOperation.value = ''
    }

    const deleteValue = () => {
      if (currentNum.value.length !== 0 && currentNum.value !== '0') {
        currentNum.value = currentNum.value.slice(0, -1)
      }
    }

    const negateValue = () => {
      if (currentNum.value === '0') {
        return (currentNum.value = '-0')
      }
      if (currentNum.value === '-') {
        return (currentNum.value = '0')
      }
      if (currentNum.value === '0.') {
        return (currentNum.value = `-${currentNum.value}`)
      }
      if (currentNum.value === '-0.') {
        return (currentNum.value = '0.')
      }
      currentNum.value = `${currentNum.value * -1}`
    }

    const handleKeydown = e => {
      pressed(e.key)
    }

    onMounted(() => window.addEventListener('keydown', handleKeydown))
    return {
      currentNum,
      pressed,
      selectedOperation,
      prevNum,
      negateValue,
      clear,
      deleteValue,
    }
  },
}
</script>

<style scoped>
.main {
  width: 100vw;
  height: 100vh;
  background: url(../assets/image/bg9.jpg);
  background-size: cover;
  background-repeat: no-repeat;
  font-family: sans-serif;
  position: fixed;
  top: 0;
  left: 0;
  display: flex;
  justify-content: center;
  align-items: center;
  font-size: 20px;
  text-align: center;
}

.calculator {
  max-width: 320px;
  background: rgba(74, 73, 73, 0.2);
  border-radius: 10px;
  justify-content: center;
  align-items: center;
  backdrop-filter: blur(5px);
  box-shadow: 0 25px 45px rgb(0, 0, 0, 0.1);
  border: 1px solid rgb(255, 255, 255, 0.5);
  border-right: 1px solid rgb(255, 255, 255, 0.2);
  border-bottom: 1px solid rgb(255, 255, 255, 0.2);
}

.buttons {
  display: flex;
  flex-wrap: wrap;
  justify-content: center;
  width: 100%;
}

.btn {
  flex: 0 0 93px;
  max-height: 63px;
  font-weight: 700;
  border: 1px solid transparent;
  line-height: 61px;
  transition: all 0.3s;
  border-radius: 3px;
  background: transparent;
  /* color: rgb(133, 131, 131); */
  color:snow;
}

.clear {
  flex: 0 0 160px;
}

.delete {
  flex: 0 0 160px;
}

.btn:hover {
  background-color: rgba(255, 255, 255, 0.3);
  border-color: rgba(255, 255, 255, 0.2);
  color: #fff;
  cursor: pointer;
  transition: all 0.2s;
}

.value {
  width: 90%;
  height: 40px;
  color: #fff;
  margin-bottom: 5px;
  margin-left: 15px;
  overflow: hidden;
}

small {
  height: 20px;
  color: #fff;
}
</style>
