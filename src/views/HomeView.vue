<template>
  <main class="main-container">
    <section>
      <h1>Market Cap</h1>
      <table class="coins-table">
        <tr class="coins-table-tr">
          <th class="th">Name</th>
          <th>Price</th>
          <th>Market Cap</th>
          <th>24h %</th>
          <th>7d %</th>
        </tr>
        <tr class="coins-table-item" v-for="(item, index) in data" :key="index">
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
import axios from 'axios'
import InputComponent from '../components/InputComponent.vue';
import ButtonComponent from '../components/ButtonComponent.vue';

export default {
  name: "HomeView",
  components: {
    InputComponent,
    ButtonComponent
  },
  data() {
    return {
      data: '',
      formatter: new Intl.NumberFormat('en-US', {
        style: 'currency',
        currency: 'USD',
      })
    }
  },
  methods: {
    async getData() {
      try {
        const data = await axios.get("http://localhost:3000/api/v1/cryptocurrency/listings/latest", {
          headers: {
            'X-CMC_PRO_API_KEY': '000470f2-3058-4f7a-88a8-b944668f4e89'
          }
        })
        console.log(data.data)
        this.data = data.data.data
      } catch (error) {
        console.log(error)
      }
    },
  },
  mounted() {
    this.getData()
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
  padding: 5px 0;
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
</style>
