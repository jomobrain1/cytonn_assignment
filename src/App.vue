<template>

  <section>
    <h1 class="center">
      Cytonn  Health Service
     <a target="_blank" href="https://github.com/jomobrain1/cytonn_assignment">
      view in github
     </a>
    </h1>
    <Tab></Tab>
    <!-- <Table></Table> -->
     <div class="bar">
      <BarChart />
     </div>
     <footer>
      &copy; 2022 jomobrain1
     </footer>
  </section>

</template>

<script>
import BarChart from './components/BarChart.vue'


import Tab from './components/Tab.vue'
export default {
  name: 'App',
  components: { BarChart,Tab}
}
</script>