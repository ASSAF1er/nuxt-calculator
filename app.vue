
<script setup>
const userInput = useState('userInput', () => ' ')
const history = ref([])

const result = ref('0')


function evaluateUserInput() {
  result.value = eval(userInput.value)
  history.value.push({ date: "October 9", expression: userInput.value, result: result.value })
}
function resetUserInput() {
  userInput.value = ''
}


</script>

<template >
  <div class="flex h-screen w-screen items-center justify-center">
    <div class="flex gap-5 rounded-xl p-5 bg-[#1b1a1f] h-4/5 w-3/5 shadow-2xl">
      <div class="w-[200px] h-full flex flex-col rounded-2xl overflow-hidden">
        <div class="bg-[#30323e] text-slate-100 px-3 py-2 tracking-wide">History</div>
        <div class="flex-1 py-3 flex flex-col justify-end bg-[#282832] overflow-y-scroll">
          <HistoryElement v-for="(item,index) in history" :key="index" :date="item.date" :expression="item.expression" :result="item.result"/>
        </div>

      </div>
      <div class="flex-1 flex  flex-col gap-5 ">
        <div
          class="w-full flex flex-col justify-end gap-5 relative h-52 p-7 rounded-2xl bg-[#30323e] text-right tracking-[3px] ">
          <div class="absolute bottom-2 left-1/2 -translate-x-1/2 w-2/5 h-[3px] bg-slate-300 rounded-full "></div>
          <p class="text-6xl text-slate-200 font-[500] ">{{ userInput }}</p>
          <p class="text-5xl text-slate-300 font-[500]">{{ result ? result : '0' }}</p>
        </div>
        <div class="flex-1 grid  grid-cols-5 grid-rows-4 gap-2 ">
          <Button value="7">7</Button>
          <Button value="8">8</Button>
          <Button value="9">9</Button>
          <Button customClass="bg-[#404558]" value="/">&divide;</Button>
          <Button customClass="bg-[#61395e]" :onclick="resetUserInput"> AC</Button>
          <Button value="4">4</Button>
          <Button value="5">5</Button>
          <Button value="6">6</Button>
          <Button customClass="bg-[#404558]" value="*">&times;</Button>
          <Button customClass="bg-[#404558]" value="()">( )</Button>
          <Button value="1">1</Button>
          <Button value="2">2</Button>
          <Button value="3">3</Button>
          <Button customClass="bg-[#404558]" value=" - ">&minus;</Button>
          <Button value='%' customClass="bg-[#404558]">%</Button>
          <Button value="0">0</Button>
          <Button value=",">.</Button>
          <Button>Del</Button>
          <Button customClass="bg-[#404558]" value="+">&plus;</Button>
          <Button customClass="bg-[#314381]" :onclick="evaluateUserInput()">&equals;</Button>

        </div>
      </div>
    </div>

  </div>
</template>
