<template>
  <main class="main-container">
    <section>
      <div class="flex">
        <div class="highlight-container">
          <HighlightsBox title="Top Gainers" :list="topGainers" />
        </div>
        <div class="highlight-container">
          <HighlightsBox title="Top Losers" :list="topLosers" />
        </div>
      </div>
      <h1>Market Cap</h1>
      <table class="coins-table">
        <tr class="coins-table-tr">
          <th>#</th>
          <th>Name</th>
          <th>Price</th>
          <th>Market Cap</th>
          <th>24h %</th>
          <th>7d %</th>
        </tr>
        <tr class="coins-table-item" v-for="(item, index) in data" :key="index">
          <td>{{ index + 1 }}</td>
          <td class="name">{{ item.name }} <span class="symbol">{{ item.symbol }}</span></td>
          <td>{{ formatter.format(item.quote.USD.price) }}</td>
          <td>{{ formatter.format(item.quote.USD.market_cap).slice(0, -3) }}</td>
          <td :class="[item.quote.USD.percent_change_24h < 0 ? 'red' : 'green']">{{
              item.quote.USD.percent_change_24h.toFixed(2)
          }}%</td>
          <td :class="[item.quote.USD.percent_change_7d < 0 ? 'red' : 'green']">{{
              item.quote.USD.percent_change_7d.toFixed(2)
          }}%</td>
        </tr>
      </table>
    </section>
  </main>
</template>

<script>
import InputComponent from '../components/InputComponent.vue';
import ButtonComponent from '../components/ButtonComponent.vue';
import HighlightsBox from '../components/HighlightsBox.vue'

export default {
  name: "HomeView",
  components: {
    InputComponent,
    ButtonComponent,
    HighlightsBox
  },
  data() {
    return {
      formatter: new Intl.NumberFormat('en-US', {
        style: 'currency',
        currency: 'USD',
      })
    }
  },
  inject: ['getData'],
  computed: {
    data() {
      return this.getData()
    },
    topGainers() {
      const list = []
      let name = ''
      let change = ''
      const sorted = this.data.sort((a, b) =>
        b.quote.USD.percent_change_24h - a.quote.USD.percent_change_24h
      )

      for (let i = 0; i < 3; i++) {
        name = sorted[i].name
        change = sorted[i].quote.USD.percent_change_24h.toFixed(2)
        list.push({ name, change })
      }

      return list
    },
    topLosers() {
      const list = []
      let name = ''
      let change = ''
      const sorted = this.data.sort((a, b) =>
        a.quote.USD.percent_change_24h - b.quote.USD.percent_change_24h
      )

      for (let i = 0; i < 3; i++) {
        name = sorted[i].name
        change = sorted[i].quote.USD.percent_change_24h.toFixed(2)
        list.push({ name, change })
      }

      return list
    },
  }
}
</script>

<style scoped>
main {
  padding: 20px 0;
}

h1 {
  margin: 20px 0;
}

table {
  border-collapse: collapse;
}

.coins-table {
  width: 100%;
}

.coins-table-tr {
  text-align: left;
}

.coins-table-tr th {
  font-weight: bold;
  padding-bottom: 5px;
}

.coins-table-item {
  font-size: 14px;
  border-top: 1px solid lightgray;
}

.coins-table .name {
  font-size: 15px;
  padding: 10px 0;
}

.coins-table .symbol {
  font-size: 16px;
  color: #999;
  margin-left: 5px;
}

.green {
  color: green;
}

.red {
  color: red;
}

.highlight-container {
  flex: 1;
}

.highlight-container:first-child {
  margin-right: 10px;
}

.highlight-container:last-child {
  margin-left: 10px;
}
</style>
