<template>
  <img
    alt="Hammurabi at his laptop"
    src="./assets/hammurabi-laptop.png"
    width="600"
  />
  <br />
  <p class="header">
    Small markov chain model made by
    <a href="https://twitter.com/willismonroe">@willismonroe</a>, header image
    by <a href="https://twitter.com/hayleybmonroe">@hayleybmonroe</a>. You can
    find the code <a href="https://github.com/willismonroe/hammurabi-markov">here</a>, thanks to the CDLI for the
    <a href="http://cdli.ucla.edu/P464358">raw text</a> of Hammurabi's laws.
    <br />
    <br />
    The button below will create laws "in-the-spirit" of Hammurabi's law-code.
    There's no guaruntee that they'll make sense, or even be complete, but
    sometimes the results are interesting.
  </p>
  <br />
  <button @click="handleClick">Generate New Laws</button>
  <br />
  <div class="laws">
    <p class="law" v-for="(law, index) in laws" :key="index">{{ law }}</p>
  </div>
</template>

<script>
import { ref, onMounted } from "vue";
import json from "@/two_markov.json";

const generate = function() {
  let newLaw = ["If", "a"];
  let prevWord = newLaw[0];
  for (let i = 1; i < 200; i++) {
    let newWord = "";
    let choices = [];
    json[newLaw[i]].forEach((x) => {
      if (x[0] === prevWord) {
        choices.push(x);
      }
    });
    newWord = choices[Math.floor(Math.random() * choices.length)][1];
    newLaw.push(newWord);
    if (
      newLaw[newLaw.length - 1][newLaw[newLaw.length - 1].length - 1] === "."
    ) {
      break;
    }
    prevWord = newLaw[newLaw.length - 2];
  }
  return newLaw;
};

export default {
  name: "App",
  setup() {
    const laws = ref([]);
    const handleClick = () => {
      laws.value = [];
      for (let i = 0; i < 5; i++) {
        laws.value.push(generate().join(" "));
      }
    };

    onMounted(() => {
      handleClick();
    });

    return { laws, handleClick };
  },
};
</script>

<style>
@import url("https://fonts.googleapis.com/css?family=Lora");

* {
  margin: 0;
  padding: 0;
  border: 0;
  font-size: 100%;
  font: inherit;
  vertical-align: baseline;
}

#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}

.header {
  margin: 0 auto;
  width: 35%;
  color: rgb(100, 100, 100);
}

button {
  padding: 1em;
  border-radius: 4px;
  box-shadow: 0 8px 16px 0 rgba(0, 0, 0, 0.2), 0 6px 20px 0 rgba(0, 0, 0, 0.19);
}

.laws {
  margin: 0 auto;
  width: 60%;
  text-align: left;
}

.law {
  margin-top: 1em;
  font-family: "Lora", serif;
  font-size: 1.5em;
}
</style>
