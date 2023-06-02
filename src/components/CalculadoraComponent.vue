<template>
  <div class="hello">
    <h1>{{ msg }}</h1>

    <table class="table table-bordered">
      <tbody>
        <tr>
          <td colspan="4">{{ output || 0 }}</td>
        </tr>
        <tr>
          <td @click="clearField">C</td>
          <td v-on:click="negativoPositivo">+/-</td>
          <td v-on:click="calculatePercent">%</td>
          <td class="right-column" v-on:click="operadores('divide')">/</td>
        </tr>
        <tr>
          <td v-on:click="tomarNumero('7')">7</td>
          <td v-on:click="tomarNumero('8')">8</td>
          <td v-on:click="tomarNumero('9')">9</td>
          <td class="right-column" v-on:click="operadores('multiply')">x</td>
        </tr>
        <tr>
          <td v-on:click="tomarNumero('4')">4</td>
          <td v-on:click="tomarNumero('5')">5</td>
          <td v-on:click="tomarNumero('6')">6</td>
          <td class="right-column" v-on:click="operadores('subtract')">-</td>
        </tr>
        <tr>
          <td v-on:click="tomarNumero('1')">1</td>
          <td v-on:click="tomarNumero('2')">2</td>
          <td v-on:click="tomarNumero('3')">3</td>
          <td class="right-column" v-on:click="operadores('add')">+</td>
        </tr>
        <tr>
          <td colspan="2" v-on:click="tomarNumero('0')">0</td>
          <td v-on:click="tomarComa()">.</td>
          <td class="right-column" v-on:click="resultado">=</td>
        </tr>
      </tbody>
    </table>
  </div>
</template>

<script>
export default {
  name: "CalculadoraComponent",
  props: {
    msg: String,
  },
  data() {
    return {
      output: "",
      previousValue: null,
      operationFired: false,
    };
  },
  methods: {
    clearField() {
      this.output = "";
    },
    calculatePercent() {
      this.output = parseFloat(this.output) / 100;
    },
    tomarNumero(number) {
      if (this.operationFired) {
        this.output = "";
        this.operationFired = false;
      }
      this.output = `${this.output}${number}`;
    },
    tomarComa() {
      if (this.output.indexOf(".") === -1) {
        this.output = this.output + ".";
      }
    },
    operadores(operation) {
      if (operation === "add") {
        this.operation = (a, b) => {
          return parseFloat(a) + parseFloat(b);
        };
      } else if (operation === "subtract") {
        this.operation = (a, b) => {
          return parseFloat(a) - parseFloat(b);
        };
      }
      if (operation === "divide") {
        this.operation = (a, b) => {
          return parseFloat(a) / parseFloat(b);
        };
      }
      if (operation === "multiply") {
        this.operation = (a, b) => {
          return parseFloat(a) * parseFloat(b);
        };
      }
      this.previousValue = this.output;
      this.operationFired = true;
    },
    resultado() {
      this.output = `${this.operation(this.previousValue, this.output)}`;
      this.previousValue = null;
    },
    negativoPositivo() {
      this.output =
        this.output[0] === "-" ? this.output.slice(1) : `-${this.output}`;
    },
  },
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
table {
  height: 200px;
  width: 200px;
}
h3 {
  margin: 30px 0 0;
}
ul {
  list-style-type: none;
  padding: 0;
}
li {
  display: inline-block;
  margin: 0 10px;
}
a {
  color: #42b960;
}

tr {
  background-image: linear-gradient(-80deg, #bff098, #6fd6ff);
}
.right-column {
  /* background-color: darkcyan; */
  /*background-image: linear-gradient(-80deg, #C6EA8D, #FE90AF);*/
  /* border-radius: 7px; */
  color: blue;
}
td:active {
  background-image: none;
  background-color: lightgray;
}
</style>
