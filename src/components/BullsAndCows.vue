<template>
  <div class="main">
    <h1 class="title">Быки и коровы</h1>
    <div class="input__wrapper">
      <input class="input" type="text" v-model="inputValue" />
      <button class="btn" @click="askNumber(inputValue)">Сделать ход</button>
    </div>
    <p class="notice" v-if="notice">{{ msg }}</p>
    <ul class="nambers">
      <li class="nambers__item" :class="{ bullOne: this.bullFlag[0] }">{{ nambers[0] }}</li>
      <li class="nambers__item" :class="{ bullTwo: this.bullFlag[1] }">{{ nambers[1] }}</li>
      <li class="nambers__item" :class="{ bullThree: this.bullFlag[2] }">{{ nambers[2] }}</li>
      <li class="nambers__item" :class="{ bullFour: this.bullFlag[3] }">{{ nambers[3] }}</li>
    </ul>
    <p>{{ inpytArray }}</p>
    <p>{{ inputValue }}</p>
    <ul class="items">
      <li class="items__value" v-for="(value, i) in inpytArray" :key="i">{{ value }}</li>
    </ul>
  </div>
</template>

<script>
export default {
  name: "BullsAndCows",
  data() {
    return {
      inputValue: "",
      inpytArray: ["2153: 1 бык, 3 коров"],
      randomNumber: "2541",
      notice: false,
      msg: "Ход - четырехзначное число",
      stepMsg: "",
      nambers: "",
      bullFlag: [{ oneisActive: false }, { twoisActive: false }, { threeisActive: false }, { fourisActive: false }],
      classesCow: ["cowOne", "cowTwo", "cowThree", "cowFour"],
    };
  },
  methods: {
    askNumber(item) {
      if (item[0] == 0) {
        this.msg = "0 не может быть первым числом";
        return (this.notice = true);
      }
      if (item.length > 4 || item.length < 4) {
        return (this.notice = true);
      }

      const parsed = parseInt(item);
      if (isNaN(parsed)) {
        return (this.notice = true);
      }
      if (!isNaN(parsed)) {
        this.notice = false;
        let bull = 0;
        let cow = 0;
        for (let i = 0; i < 4; i++) {
          if (item[i] == item[i + 1]) {
            this.msg = "Цифры не должны повторяться";
            return (this.notice = true);
          }
          if (this.randomNumber[i] == item[i]) {
            bull++;
            this.bullFlag[i] = false;
            console.log(this.bullFlag);
          }
          for (let j = 0; j < i; j++) {
            if (this.randomNumber[i] == item[j]) {
              cow++;
            }
          }
        }
        this.stepMsg = `${item}: ${bull} бык, ${cow} коров`;
        this.nambers = this.inputValue;
        this.inputValue = "";
        return this.inpytArray.unshift(this.stepMsg);
      }
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
.title {
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
