<script setup>
import { ref } from 'vue'
import Widget from './components/Widget.vue'

const mainContainer = ref(null)

const handleWidgetMoved = ({x, y}, position) => {
  [...mainContainer.value.children].forEach((widget, index) => {
    if (index === position) return
    const {top, right, bottom, left} = widget.getBoundingClientRect()
    const dropWidget = widgets.value[position]
    if (index === 0 && x <= left && bottom >= y) {
      widgets.value.splice(position, 1)
      widgets.value.splice(index, 0, dropWidget)
    }
    else if (left <= x && x <= right && top <= y && y <= bottom) {
      widgets.value.splice(position, 1)
      widgets.value.splice(index, 0, dropWidget)
    }
  })
}

const widgets = ref([
  {
    id: 0,
    classes: 'col-3 bg-red'
  },
  {
    id: 1,
    classes: 'col-6 bg-green'
  },
  {
    id: 2,
    classes: 'col-9 bg-blue'
  },
  {
    id: 3,
    classes: 'col-12 bg-red'
  },
  {
    id: 4,
    classes: 'col-3 bg-green'
  },
  {
    id: 5,
    classes: 'col-3 bg-blue'
  },
  {
    id: 6,
    classes: 'col-6 bg-red'
  },
  {
    id: 7,
    classes: 'col-3 bg-green'
  },
  {
    id: 8,
    classes: 'col-3 bg-red'
  },
  {
    id: 9,
    classes: 'col-3 bg-green'
  },
  {
    id: 10,
    classes: 'col-3 bg-blue'
  }
])
</script>

<template>
  <div
    ref="mainContainer"
    id="main-container">

    <Widget
      v-for="({id, classes}, index) in widgets"
      :class="classes"
      :key="id"
      @rearrange="coords => handleWidgetMoved(coords, index)">
      Test Widget
      <h1>
        {{ id }}
      </h1>
    </Widget>

  </div>
</template>

<style>
* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}

body {
  background-color: #123;
}

#main-container {
  display: flex;
  align-items: flex-start;
  flex-wrap: wrap;
  gap: 20px;
  width: 100%;
  max-height: 100vh;
  padding: 30px;
  overflow-y: auto;
}

.col-3 {
  width: calc(25% - 20px);
}
.col-6 {
  width: calc(50% - 20px);
}
.col-9 {
  width: calc(75% - 20px);
}
.col-12 {
  width: calc(100% - 20px);
}

.bg-red {
  background-color: #f99;
}
.bg-green {
  background-color: #9f9;
}
.bg-blue {
  background-color: #7af;
}
</style>