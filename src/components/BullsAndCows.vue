<template>
  <div class="main" @load="getRandomArbitrary">
    <h1 class="title">Быки и коровы</h1>
    <p class="subtitle">Компьютер уже что-то задумал. Играем!</p>
    <div class="input__wrapper">
      <input class="input" type="text" v-model="inputValue" @keyup.enter="askNumber(inputValue)" />
      <button class="btn" @click="askNumber(inputValue)">Сделать ход</button>
      <button class="btn" @click="newGame">Новая игра</button>
    </div>
    <p class="active-cow cow p-cow">Желтые - Коровы!</p>
    <br />
    <p class="active-bull bull p-bull">Зеленые - Быки!</p>
    <br />
    <p class="notice" v-if="notice">{{ msg }}</p>
    <ul class="nambers" v-if="String(nambersInCircle).length <= 4">
      <li class="nambers__item bull cow" v-for="(item, i) in nambersInCircle" :key="i">{{ item }}</li>
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
// function bullCount(number, compare) {
//   number = String(number);
//   compare = String(compare);
//   let bull = 0;

//   for (let i = 0; i < number.length; i++) {
//     if (number[i] == compare[i]) {
//       bull++;
//     }
//   }
//   return bull;
// }
// Считаем коров
// function cowCount(number, compare) {
//   number = String(number);
//   compare = String(compare);
//   let cow = 0;

//   for (let i = 0; i < number.length; i++) {
//     for (let j = 0; j < number.length; j++) {
//       if (number[i] == compare[j]) {
//         cow++;
//       }
//     }
//   }
//   return cow;
// }
function removeClass() {
  let allItem = document.querySelectorAll(".nambers__item");
  for (let i = 0; i < allItem.length; i++) {
    allItem[i].classList.remove("active-bull");
    allItem[i].classList.remove("active-cow");
  }
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
      nambersInCircle: undefined,
    };
  },
  methods: {
    async askNumber(item) {
      this.nambersInCircle = item;
      await this.$nextTick();
      let numItem = document.querySelectorAll(".nambers__item");
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
        let bull = 0;
        let cow = 0;
        removeClass();
        for (let i = 0; i < this.randomNumber.length; i++) {
          console.log(this.randomNumber + ":" + this.randomNumber[i]);
          if (this.randomNumber[i] == item[i]) {
            bull++;
            numItem[i].classList.add("active-bull");
          }
          for (let j = 0; j < this.randomNumber.length; j++) {
            if (this.randomNumber[i] == item[j]) {
              cow++;
              numItem[j].classList.add("active-cow");
            }
          }
        }

        // Конец игры
        if (bull === 4) {
          this.msg = "Ты Победил!";
          this.notice = true;

          this.stepMsg = `${item}: ${bull} бык, ${cow} коров`;
          this.inputValue = "";
          return this.inpytArray.unshift(this.stepMsg);
        }
        cow = cow - bull;
        // Добовляем данные в массив
        this.stepMsg = `${item}: ${bull} бык, ${cow} коров`;
        this.inputValue = "";
        return this.inpytArray.unshift(this.stepMsg);
      }
    },
    newGame() {
      this.inputValue = "";
      this.inpytArray.length = 0;
      this.randomNumber = numberFromArray();
      this.notice = false;
      this.nambersInCircle = undefined;
      this.msg = "";
      this.stepMsg = "";
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
.notice,
.p-cow,
.p-bull {
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
.active-cow.cow {
  background: yellow;
}
.active-bull.bull {
  background: green;
}
.p-cow,
.p-bull {
  display: inline-block;
}
</style>
