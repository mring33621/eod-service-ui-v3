<template>
  <div>
    <select v-model="selectedExchange" @change="fetchSymbols">
      <option v-for="exchange in exchanges" :key="exchange" :value="exchange">
        {{ exchange }}
      </option>
    </select>
    <select v-model="selectedSymbol" @change="fetchPoints">
      <option v-for="symbol in symbols" :key="symbol" :value="symbol">
        {{ symbol }}
      </option>
    </select>
    <table>
      <thead>
      <tr>
        <th>Date</th>
        <th>Symbol</th>
        <th>Open</th>
        <th>High</th>
        <th>Low</th>
        <th>Close</th>
        <th>Volume</th>
      </tr>
      </thead>
      <tbody>
      <tr v-for="point in points" :key="point.date">
        <td>{{ point.date }}</td>
        <td>{{ point.symbol }}</td>
        <td>{{ point.open }}</td>
        <td>{{ point.high }}</td>
        <td>{{ point.low }}</td>
        <td>{{ point.close }}</td>
        <td>{{ point.volume }}</td>
      </tr>
      </tbody>
    </table>
  </div>
</template>

<script>
import axios from 'axios'

export default {
  data() {
    return {
      exchanges: [],
      selectedExchange: '',
      symbols: [],
      selectedSymbol: '',
      points: []
    }
  },
  mounted() {
    this.fetchExchanges()
  },
  watch: {
    selectedExchange() {
      this.points = [] // clear points when exchange changes
    }
  },
  methods: {
    async fetchExchanges() {
      try {
        const response = await axios.get('/md/exchanges')
        this.exchanges = response.data
      } catch (error) {
        console.error(error)
      }
    },
    async fetchSymbols() {
      if (this.selectedExchange) {
        try {
          const response = await axios.get(`/md/${this.selectedExchange}/top10symbols`)
          this.symbols = response.data
        } catch (error) {
          console.error(error)
        }
      } else {
        this.symbols = []
      }
    },
    async fetchPoints() {
      if (this.selectedSymbol) {
        try {
          const response = await axios.get(`/md/${this.selectedSymbol}/points`)
          this.points = response.data
        } catch (error) {
          console.error(error)
        }
      } else {
        this.points = []
      }
    }
  }
}
</script>