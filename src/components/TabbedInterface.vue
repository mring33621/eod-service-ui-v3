<template>
  <div>
    <ul>
      <li v-for="tab in tabs" :key="tab.name">
        <a :href="`#${tab.name}`" @click.prevent="selectTab(tab)">{{ tab.name }}</a>
      </li>
    </ul>
    <component :is="selectedTabComponent" />
  </div>
</template>

<script>
import { ref, computed } from 'vue'
import Component1 from './HelloWorld.vue'
import Component2 from './HistoricMarketData.vue'

export default {
  setup() {
    const tabs = ref([
      { name: 'IntroPage' },
      { name: 'MarketData' }
    ])
    const selectedTab = ref('IntroPage')
    const selectedTabComponent = computed(() => {
      return selectedTab.value === 'IntroPage' ? Component1 : Component2
    })

    const selectTab = (tab) => {
      selectedTab.value = tab.name
    }

    return {
      tabs,
      selectTab,
      selectedTabComponent
    }
  }
}
</script>