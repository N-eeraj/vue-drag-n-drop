<script setup>
import { onMounted, ref } from 'vue'
import WidgetBoard from './components/WidgetBoard.vue'
import Widget from './components/Widget.vue'
import Chart from './components/Chart.vue'
import Table from './components/Table.vue'
import Graph from './components/Graph.vue'

import data from './data'

const mainContainer = ref(null)

const handleRemoveWidget = index => {
  widgets.value.splice(index, 1)
  console.log(widgets.value)
}

const handleWidgetMoved = ({x, y}, position) => {
  let completed = false
  let endPosition = null;

  [...mainContainer.value.$el.children].forEach((widget, index, array) => {
    // omit the dragged widget
    if (index === position || completed) return

    // get the boundary of the widget
    const {top, right, bottom, left} = widget.getBoundingClientRect()
    const dropWidget = widgets.value[position]

    // check if the widget is being dragged to start of the widget board
    if (index === 0 && x <= left && bottom >= y) {
      widgets.value.splice(position, 1)
      widgets.value.splice(index, 0, dropWidget)
      completed = true
    }
    // check if the widget is being dragged to end of the widget board
    else if (index === (array.length - 1) && x >= left && top <= y) {
      widgets.value.splice(position, 1)
      widgets.value.splice(index, 0, dropWidget)
      completed = true
    }
    // check if the widget is being dragged on top of another
    else if (left <= x && x <= right && top <= y && y <= bottom) {
      widgets.value.splice(position, 1)
      widgets.value.splice(index, 0, dropWidget)
      completed = true
    }
    // check if a widget is dropped in between others
    else if (top <= y && y <= bottom && x <= left && array[index-1].getBoundingClientRect().right <= x) {
      widgets.value.splice(position, 1)
      widgets.value.splice(index < position ? index : index - 1, 0, dropWidget)
      completed = true
    }
    // check if widget is moved to start of a row
    else if (x <= left  && top <= y && y <= bottom) {
      widgets.value.splice(position, 1)
      widgets.value.splice(index, 0, dropWidget)
      completed = true
    }
    // check if widget is moved to end of a row
    else if (right <= x  && top <= y && y <= bottom) {
      endPosition = index < position ? index + 1 : index
    }
  })

  // handle movement when widget is moved to the end of a row
  if (!completed) {
    const dropWidget = widgets.value[position]
    widgets.value.splice(position, 1)
    widgets.value.splice(endPosition, 0, dropWidget)
  }

  console.log(widgets.value)
}

const widgets = ref([])

const getWidget = widget => {
  switch (widget) {
    case 'chart':
      return Chart
    case 'table':
      return Table
    case 'graph':
      return Graph
  }
}

onMounted(() => {
  widgets.value = data
})
</script>

<template>
  <WidgetBoard ref="mainContainer">

    <Widget
      v-for="({widget, cols}, index) in widgets"
      :cols="cols"
      :key="index"
      @rearrange="coords => handleWidgetMoved(coords, index)"
      @remove="handleRemoveWidget(index)">
      <component v-bind:is="getWidget(widget)" />
    </Widget>


  </WidgetBoard>
</template>

<style>
* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
  font-family: sans-serif;
}

body {
  background-color: #123;
}
</style>