<script setup>
import { ref,onMounted ,computed} from 'vue'
const list = [
  {
    name: 'ailson',
    sex: 'male',
  },
  {
    name: 'alex',
    sex: 'male',
  },
  {
    name: 'ariel',
    sex: 'female',
  },
  {
    name: 'bubu',
    sex: 'male',
  },
  {
    name: 'christine',
    sex: 'female',
  },
  {
    name: 'connie',
    sex: 'female',
  },
  {
    name: 'coreen',
    sex: 'female',
  },
  {
    name: 'elena',
    sex: 'female',
  },
  {
    name: 'eric wang',
    sex: 'male',
  },
  {
    name: 'eric chen',
    sex: 'male',
  },
  {
    name: 'kiki',
    sex: 'female',
  },
  {
    name: 'jenny',
    sex: 'female',
  },
  {
    name: 'josephine',
    sex: 'female',
  },
  {
    name: 'judy',
    sex: 'female',
  },
  {
    name: 'arthur',
    sex: 'male',
  },
  {
    name: 'kazumi',
    sex: 'female'
  },
  {
    name: 'marshall',
    sex: 'male',
  },
  {
    name: 'nicole',
    sex: 'female'
  },
  {
    name: 'rarity',
    sex: 'female',
  },
  {
    name: 'robin',
    sex: 'male',
  },
  {
    name: 'rosalie',
    sex: 'female',
  },
  {
    name: 'sandy',
    sex: 'female',
  },
  {
    name: 'sean',
    sex: 'male',
  },
  {
    name: 'steve',
    sex: 'male',
  },
  {
    name: 'summer',
    sex: 'female',
  },
  {
    name: 'taco',
    sex: 'male',
  },
  {
    name: 'tina',
    sex: 'female',
  },
  {
    name: 'vin',
    sex: 'male',
  },
  {
    name: '樺樺',
    sex: 'female',
  },
  {
    name: '妍蓁',
    sex: 'female',
  },
  {
    name: '羊咩',
    sex: 'female',
  },
  {
    name: '育汝',
    sex: 'female',
  },
  {
    name: '俊杰',
    sex: 'male',
  },
  {
    name: '閃亮亮',
    sex: 'male',
  },
]

const orders = ref([])
const maleCount = ref(0)
const femaleCount = ref(0)
const maleParticipant = computed(()=>{
  return list.filter(person=> person.sex === 'male').length
})
const femaleParticipant = computed(()=>{
  return list.filter(person=> person.sex === 'female').length
})

function reorderPeople() {

  function addSexCount(person){
    if(person.sex === 'female'){
        femaleCount.value++  
      }else{
        maleCount.value++
      }
  }

  while (orders.value.length !== list.length) {
    // 第一位就隨機抽
    if(orders.value.length === 0){
      const randomIndex = Math.floor(Math.random() * list.length)
      const newPerson = list[randomIndex]
      addSexCount(newPerson)
      orders.value.push(newPerson)
      continue;
    }
    
    // Filter out people already in orders
    const availablePeople = list.filter(person => 
      !orders.value.find(p => p.name === person.name)
    )
    const femalePeople = availablePeople.filter(person => person.sex === 'female')
    const malePeople = availablePeople.filter(person => person.sex === 'male')

    // 如果女生已連續抽3位，且還有男生的話，強制下一個為男生
    if(femaleCount.value === 3 && malePeople.length !== 0){
      const randomIndex = Math.floor(Math.random() * malePeople.length)
      orders.value.push(malePeople[randomIndex])
      maleCount.value++
      femaleCount.value = 0
      continue;
    }
 
    // 如果男生已連續抽2位，且還有女生的話，強制下一個為女生
    if(maleCount.value == 2 && femalePeople.length !== 0){
      const randomIndex = Math.floor(Math.random() * femalePeople.length)
      orders.value.push(femalePeople[randomIndex])
      femaleCount.value++
      maleCount.value = 0
      continue;
    }

    if(orders.value[orders.value.length - 1].sex === 'male') {
      // 如果上一位是男生，增加下一位是女生的機率
      const newAvailablePeople = [...femalePeople,...femalePeople,...malePeople]
      // Add random person from available people
      const randomIndex = Math.floor(Math.random() * newAvailablePeople.length)
      const newPerson = newAvailablePeople[randomIndex]
      addSexCount(newPerson)
      orders.value.push(newPerson)
    }else{
      // 如果上一位是女生，就照一般方式走
      const randomIndex = Math.floor(Math.random() * availablePeople.length)
      const newPerson = availablePeople[randomIndex]
      addSexCount(newPerson)
      orders.value.push(newPerson)
    }
  }
}

onMounted(() => {
  reorderPeople()
})
</script>

<template>
  <div>
    <div>男生參加人數：{{ maleParticipant }}</div>
    <div>女生參加人數：{{ femaleParticipant }}</div>
    <ul>
      <li v-for="item in orders" :key="item.name" :class="item.sex">
        {{ item.name }}
      </li>
    </ul>
  </div>
  
</template>

<style scoped>
*{
  color: white
}
li{
  text-align: left;
}
li.male{
  color: aqua
}
li.female{
  color: deeppink
}
</style>
