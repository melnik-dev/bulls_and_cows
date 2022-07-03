<template>
  <div class="main" @load="getRandomArbitrary">
    <h1 class="title">Быки и коровы</h1>
    <p class="subtitle">Компьютер уже что-то задумал. Играем!</p>
    <div class="input__wrapper">
      <input class="input" type="text" v-model="inputValue" @keyup.enter="askNumber(inputValue)" />
      <button class="btn" @click="askNumber(inputValue)">Сделать ход</button>
      <button class="btn" @click="newGame">Новая игра</button>
    </div>
    <p class="notice" v-if="notice">{{ msg }}</p>
    <ul class="nambers">
      <li class="nambers__item" v-for="(item, i) in nambersInCircle" :key="i" :class="classesBull[i]">{{ item }}</li>
    </ul>
    <p style="display: none">random Number: {{ randomNumber }}</p>
    <ul class="items">
      <li class="items__value" v-for="(value, i) in inpytArray" :key="i">{{ value }}</li>
    </ul>
  </div>
</template>

<script>
// Масив рандомных чисел
function getRandomNumArray() {
  let randomNumArray = [];
  for (let i = 0; i <= 10; i++) {
    let num = String(Math.floor(Math.random() * (1000 - 9999)) + 9999);
    randomNumArray.push(num);
  }
  return randomNumArray;
}
// Уникальность в числе
function getUniqueNum(num) {
  num = String(num);
  let set = new Set();

  for (let i = 0; i < num.length; i++) {
    set.add(num[i]);
    if (set.size == 4) {
      return true;
    }
  }

  return false;
}
// Масив уникальных из рандомных чисел
function getUniqueNumArray(array) {
  let uniqueNumArray = [];
  for (let i = 0; i < array.length; i++) {
    let element = array[i];
    if (getUniqueNum(element)) {
      uniqueNumArray.push(element);
    }
  }
  return uniqueNumArray;
}
// Первое число из масива иникальных чисел
function numberFromArray() {
  let num = getUniqueNumArray(getRandomNumArray());
  return num[0];
}
// Считаем быков
function bullCount(number, compare) {
  number = String(number);
  compare = String(compare);
  let bull = 0;

  for (let i = 0; i < number.length; i++) {
    if (number[i] == compare[i]) {
      bull++;
    }
  }
  return bull;
}
// Считаем коров
function cowCount(number, compare) {
  number = String(number);
  compare = String(compare);
  let cow = 0;

  for (let i = 0; i < number.length; i++) {
    for (let j = 0; j < number.length; j++) {
      if (number[i] == compare[j]) {
        cow++;
      }
    }
  }
  return cow;
}
export default {
  name: "BullsAndCows",
  data() {
    return {
      inputValue: "",
      inpytArray: [],
      randomNumber: numberFromArray(),
      notice: false,
      msg: "",
      stepMsg: "",
      nambersInCircle: "",
      classesBull: [{ "bull-one": this.bullOneisActive }, { bullTwo: this.bullTwoisActive }, { bullThree: this.bullThreeisActive }, { bullFour: this.bullFourisActive }],
      classesCow: ["cowOne", "cowTwo", "cowThree", "cowFour"],
      bullOneisActive: true,
      bullTwoisActive: false,
      bullThreeisActive: false,
      bullFourisActive: false,
    };
  },
  methods: {
    askNumber(item) {
      // Проверка первого числа неравно 0
      if (item[0] == 0) {
        this.msg = "0 не может быть первым числом";
        return (this.notice = true);
      }
      // Проверка длины числа
      if (item.length > 4 || item.length < 4) {
        this.msg = "Ход - четырехзначное число";
        return (this.notice = true);
      }
      // Числа в числе не повторяются
      if (!getUniqueNum(item)) {
        this.msg = "Цифры не должны повторяться";
        return (this.notice = true);
      }

      const parsed = parseInt(item);
      // Проверка число на строку
      if (isNaN(parsed)) {
        this.msg = "Ход - четырехзначное число";
        return (this.notice = true);
      }
      // Проверка числа и подсчет
      if (!isNaN(parsed)) {
        this.notice = false;
        let bull = bullCount(this.randomNumber, item);
        let cow = cowCount(this.randomNumber, item) - bull;
        // Конец игры
        if (bull === 4) {
          this.msg = "Ты Победил!";
          this.notice = true;

          this.stepMsg = `${item}: ${bull} бык, ${cow} коров`;
          this.nambersInCircle = this.inputValue;
          this.inputValue = "";
          return this.inpytArray.unshift(this.stepMsg);
        }

        // Добовляем данные в массив
        this.stepMsg = `${item}: ${bull} бык, ${cow} коров`;
        this.nambersInCircle = this.inputValue;
        this.inputValue = "";
        return this.inpytArray.unshift(this.stepMsg);
      }
    },
    newGame() {
      this.inputValue = "";
      this.inpytArray.length = 0;
      this.randomNumber = numberFromArray();
      this.notice = false;
      this.nambersInCircle = "";
      this.msg = "";
    },
  },
};
</script>
<style scoped>
.main {
  width: 400px;
  margin: 0 auto;
  padding-top: 60px;
  font-size: 18px;
}
.title,
.subtitle {
  text-align: center;
}
.input__wrapper {
  display: flex;
}
.input {
  flex: 1;
  padding: 6px;
  border: 1px solid lightgray;
  border-radius: 6px;
}
.btn {
  border: 1px solid lightgray;
  border-radius: 6px;
  margin-left: 6px;
  cursor: pointer;
}
.items {
  list-style: none;
  padding: 0;
}
.items__value {
  padding: 6px 0;
}
.notice {
  font-size: 14px;
  background: rgb(248, 215, 218);
  color: rgb(114, 28, 36);
  padding: 4px;
  border-radius: 6px;
  display: inline-block;
}
.nambers {
  list-style: none;
  padding: 0;
  display: flex;
  justify-content: center;
  gap: 20px;
}
.nambers__item {
  font-size: 26px;
  background: lightblue;
  width: 40px;
  height: 40px;
  border-radius: 50%;
  text-align: center;
  line-height: 40px;
}
.cowOne,
.cowTwo,
.cowThree,
.cowFour {
  background: yellow;
}
.bullOne,
.bullTwo,
.bullThree,
.bullFour {
  background: red;
}
</style>
