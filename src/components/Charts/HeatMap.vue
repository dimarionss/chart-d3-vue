<template>
  <div class="scroll-container">
    <div id="my_dataviz"></div>
  </div>
</template>

<script setup lang="ts">
import { onMounted, defineProps, watch } from 'vue'
import * as d3 from "d3"

const props = defineProps({ dataChart: Array })

onMounted(() => {
  const margin = { top: 80, right: 0, bottom: 30, left: 180 }
  const minCellSize = 80
  const maxCellSize = 80
  const cellPadding = 7

  const svg = d3.select("#my_dataviz")
    .append("svg")
    .attr("width", 0)
    .attr("height", 0)
    .append("g")
    .attr("transform", `translate(${margin.left}, ${margin.top})`)

  const colorScale = d3.scaleThreshold()
    .domain([10, 20, 30, 40, 50, 60, 70, 80, 90, 100])
    .range(["#FF3E41", "#FF4D80", "#F6D4BA", "#14248A", "#BC69AA", "#C585B3", "#00FFCD", "#A0EADE"])

  const updateChart = (data) => {
    svg.selectAll("*").remove()

    const myGroups = Array.from(new Set(data.map(d => d.col)))
    const myVars = Array.from(new Set(data.map(d => d.row)))

    const cellWidth = Math.max(minCellSize, Math.min(maxCellSize, (window.innerWidth - margin.left - margin.right) / myGroups.length - cellPadding))
    const cellHeight = Math.max(minCellSize, Math.min(maxCellSize, (window.innerHeight - margin.top - margin.bottom) / myVars.length - cellPadding))

    const svgWidth = (cellWidth + cellPadding) * myGroups.length + margin.left + margin.right
    const svgHeight = (cellHeight + cellPadding) * myVars.length + margin.top + margin.bottom

    d3.select("#my_dataviz svg")
      .attr("width", svgWidth)
      .attr("height", svgHeight)

    const x = d3.scaleBand()
      .range([0, cellWidth * myGroups.length + cellPadding * (myGroups.length - 1)])
      .domain(myGroups)
      .padding(0.05)

    svg.append("g")
      .style("font-size", 18)
      .style("color", "grey")
      .call(d3.axisTop(x).tickSize(0))
      .select(".domain").remove()

    const y = d3.scaleBand()
      .range([cellHeight * myVars.length + cellPadding * (myVars.length - 1), 0])
      .domain(myVars)
      .padding(0.05)

    svg.append("g")
      .style("font-size", 18)
      .style("color", "grey")
      .call(d3.axisLeft(y).tickSize(0))
      .select(".domain").remove()

    svg.selectAll()
      .data(data, d => d.col + ':' + d.row)
      .join("rect")
      .attr("x", d => x(d.col))
      .attr("y", d => y(d.row))
      .attr("width", cellWidth)
      .attr("height", cellHeight)
      .attr("rx", 10)
      .attr("ry", 10)
      .style("fill", d => colorScale(d.value))
      .style("stroke-width", 2)
      .style("stroke", "none")
      .style("opacity", 0.8)
  }

  watch(() => props.dataChart, (newData) => {
    updateChart(newData)
  }, { immediate: true })

  updateChart(props.dataChart)
})
</script>

<style scoped>
.scroll-container {
  width: 100%;
  height: 600px;
  overflow-y: auto;
}
</style>