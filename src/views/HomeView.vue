<template>
  <main class="main-container">
    <section>
      <h1>Market Cap</h1>
      <table class="coins-table">
        <tr class="coins-table-tr">
          <th>Name</th>
          <th>Price</th>
          <th>Market Cap</th>
          <th>24h %</th>
          <th>7d %</th>
        </tr>
        <tr v-for="(item, index) in data" :key="index">
          <td>{{ item.name }} {{ item.symbol }}</td>
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

.coins-table {
  width: 100%;
}

.coins-table-tr {
  text-align: left;
}

.coins-table-tr th {
  font-weight: bold;
}

.green {
  color: green;
}

.red {
  color: red;
}
</style>
