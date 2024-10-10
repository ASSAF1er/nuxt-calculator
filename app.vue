
<script setup >

const userInput = useState('userInput', () => '0')
const history = ref([])
onMounted(() => {
  history.value = JSON.parse(localStorage?.getItem('history')) || []

})

const result = ref('')
const pressedEqual = ref(false)



function addDigit(value) {
  if (userInput.value.trim() === '0' || pressedEqual.value) {
    userInput.value = value
    result.value = '0'
  }
  else { userInput.value += value }
  pressedEqual.value = false
}

function backspace() {
  userInput.value = userInput.value.slice(0, -1)
}

function addOperator(symbol) {
  if (pressedEqual.value) {
    userInput.value = result.value
  }
  userInput.value += symbol
  pressedEqual.value = false
}

function addBrackets() {
  if (userInput.value.trim() === '0' || pressedEqual.value) {
    userInput.value = ''
    result.value = '0'
  }

  const lastBracket = userInput.value.split('').reverse().find(item => (item == '(' || item == ')'))
  if (lastBracket === '(') {
    userInput.value += ')'
  }
  else {
    userInput.value += '('
  }
}

function addToHistory(calculation) {
  history.value.unshift({ date: "October 9", expression: userInput.value, result: result.value })
  localStorage?.setItem('history', JSON.stringify(history.value))
}

function evaluateUserInput() {
  try {
    result.value = eval(userInput.value)
    addToHistory()
    pressedEqual.value = true
  } catch {
    result.value = "Syntax Error"
  }

}

function resetUserInput() {
  userInput.value = '0'
  result.value = "0"
}
function clearHistory() {
  localStorage?.removeItem('history')
  history.value = []
}



// Listen keyboard events
document?.addEventListener('keydown', function (event) {
  const key = event.key; // key pressed

  if (['+', '-', '*', '/', '=', 'Enter', 'Backspace'].includes(key)) {
    switch (key) {
      case 'Enter':
        event.preventDefault();
        evaluateUserInput()
        break;
      case '=':
        evaluateUserInput()
        break;
      case 'Backspace':
        backspace()
        break;
      default:
        addOperator(key);
        break;
    }
  }
  if (key >= '0' && key <= '9') {
    addDigit(key)
  }
  if (['(', ')'].includes(key)) {
    addBrackets()
  }
});


</script>

<template >
  <div class="flex h-screen w-screen items-center justify-center">
    <div class="flex gap-5 rounded-xl p-5 bg-[#1b1a1f] h-4/5 w-3/5 shadow-2xl">
      <div class="w-[200px] h-full flex flex-col rounded-2xl overflow-hidden">
        <div class=" flex justify-between items-center w-full bg-[#30323e] text-slate-100 px-3 py-2 tracking-wide">
          <span>History </span>
           <button
           @click="clearHistory"
            class="border border-gray-600 px-2 rounded-md text-gray-300 text-sm hover:bg-red-500 hover:text-white active:scale-95 duration-300 ease-in-out ">clear</button>
        </div>
        <div class="flex-1 flex flex-col justify-end py-3  bg-[#282832]">
          <div className="flex flex-col justify-end  max-h-[200px]  overflow-y-scroll">
            <span v-if="history.length==0" class="p-2 text-sm text-gray-400">No calculations yet.</span>
            <HistoryElement v-for="(item, index) in history" :key="index" :date="item.date" :expression="item.expression"
              :result="item.result" />
          </div>
        </div>

      </div>
      <div class="flex-1 flex  flex-col gap-5 ">
        <div
          class="w-full flex flex-col justify-end gap-5 relative h-52 p-7 rounded-2xl bg-[#30323e] text-right tracking-[3px] ">
          <div class="absolute bottom-2 left-1/2 -translate-x-1/2 w-2/5 h-[3px] bg-slate-300 rounded-full "></div>
          <input v-model="userInput" disabled class="text-6xl  text-slate-200 font-[500] w-full bg-transparent text-right focus:outline-none" />
          <input v-model="result" disabled class="text-5xl text-ellipsis text-slate-300 font-[500] w-full bg-transparent text-right focus:outline-none"/>
        </div>
        <div class="flex-1 grid  grid-cols-5 grid-rows-4 gap-2 ">
          <Button :onclick="() => addDigit('7')">7</Button>
          <Button :onclick="() => addDigit('8')">8</Button>
          <Button :onclick="() => addDigit('9')">9</Button>
          <Button :onclick="() => addOperator('/')" customClass="bg-[#404558]">&divide;</Button>
          <Button :onclick="resetUserInput" customClass="bg-[#61395e]"> AC</Button>
          <Button :onclick="() => addDigit('4')">4</Button>
          <Button :onclick="() => addDigit('5')">5</Button>
          <Button :onclick="() => addDigit('6')">6</Button>
          <Button :onclick="() => addOperator('*')" customClass="bg-[#404558]">&times;</Button>
          <Button :onclick="addBrackets" customClass="bg-[#404558]">( )</Button>
          <Button :onclick="() => addDigit('1')">1</Button>
          <Button :onclick="() => addDigit('2')">2</Button>
          <Button :onclick="() => addDigit('3')">3</Button>
          <Button :onclick="() => addOperator('-')" customClass="bg-[#404558]">&minus;</Button>
          <Button :onclick="() => addOperator('%')" customClass="bg-[#404558]">%</Button>
          <Button :onclick="() => addDigit('0')">0</Button>
          <Button :onclick="() => addDigit('7')">.</Button>
          <Button :onclick="backspace">Del</Button>
          <Button :onclick="() => addOperator('+')" customClass="bg-[#404558]">&plus;</Button>
          <Button :onclick="() => evaluateUserInput()" customClass="bg-[#314381]">&equals;</Button>

        </div>
      </div>
    </div>

  </div></template>
