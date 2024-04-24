// App.vue
<template>
  <div>
    <TabbedInterface />
  </div>
</template>

<script>
import { defineComponent } from 'vue'
import TabbedInterface from './components/TabbedInterface.vue'

export default defineComponent({
  components: { TabbedInterface }
})
</script>