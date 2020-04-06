<template>
  <div id="app">
    <h1>Dinner calculator</h1>
    <div>
      <table>
        <tbody>
          <tr>
            <td>Dinner</td>
            <td>{{ dinner | currency("€", 0) }}</td>
            <td>
              <button @click="updateDinner('dinner', 5)">+</button>
              <button @click="updateDinner('dinner', -5)">-</button>
            </td>
          </tr>
          <tr>
            <td>Tip</td>
            <td>{{ tip | currency("€", 0) }}</td>
            <td>
              <button @click="updateDinner('tip', 1)">+</button>
              <button @click="updateDinner('tip', -1)">-</button>
            </td>
          </tr>
          <tr>
            <td>People</td>
            <td>{{ people }}</td>
            <td>
              <button @click="updateDinner('people', 1)">+</button>
              <button @click="updateDinner('people', -1, 1)">-</button>
            </td>
          </tr>
          <tr>
            <td>Total with taxes ({{ TAXES | percent }})</td>
            <td>{{ totalWithTaxes | currency("€") }}</td>
            <td></td>
          </tr>
          <tr>
            <td>Total with tip</td>
            <td>{{ totalWithTip | currency("€") }}</td>
            <td></td>
          </tr>
          <tr>
            <td>Total per person</td>
            <td>{{ totalPerPerson | currency("€") }}</td>
            <td></td>
          </tr>
        </tbody>
      </table>
    </div>
  </div>
</template>

<script>
export default {
  name: "App",
  data() {
    return {
      dinner: 0,
      tip: 0,
      people: 1,
      TAXES: 0.21,
    };
  },
  computed: {
    totalWithTaxes() {
      return this.dinner + this.dinner * this.TAXES;
    },
    totalWithTip() {
      return this.totalWithTaxes + this.tip;
    },
    totalPerPerson() {
      return this.totalWithTip / this.people;
    },
  },
  methods: {
    updateDinner(variableToChange, increment, min = 0) {
      this[variableToChange] = this[variableToChange] + increment;

      if (this[variableToChange] < min) {
        this[variableToChange] = min;
      }
    },
  },
  filters: {
    currency(value, currencySymbol, decimalDigits = 2) {
      return `${value.toFixed(decimalDigits)} ${currencySymbol}`;
    },
    percent(value) {
      return `${value * 100}%`;
    },
  },
};
</script>

<style lang="scss">
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}

/* General */

h1 {
  text-align: center;
  margin-top: 40px;
}

/* Table */

table {
  margin: 25px auto;
  border-collapse: collapse;
  border: 1px solid #eee;
  border-bottom: 2px solid #42b883;
  box-shadow: 0px 0px 20px rgba(0, 0, 0, 0.1), 0px 10px 20px rgba(0, 0, 0, 0.05),
    0px 20px 20px rgba(0, 0, 0, 0.05), 0px 30px 20px rgba(0, 0, 0, 0.05);

  tr:hover {
    background: #f4f4f4;
  }

  tr:hover td {
    color: #555;
  }

  td {
    color: #999;
    border: 1px solid #eee;
    padding: 12px 35px;
    border-collapse: collapse;
  }

  tr td:nth-child(2) {
    text-align: right;
  }
}

/* Buttons */

button {
  padding: 4px 7px;
  border-radius: 1px;
  outline: none;
}
</style>
