<script setup>
defineProps({
    cols: {
        type: Number,
        required: false,
        default: 3
    }
})
const emit = defineEmits([
    'rearrange',
    'remove'
])

const handleDragStart = ({target}) => {
    target.classList.add('dragging')
}
const handleTouchStart = event => {
    event.preventDefault()
    handleDragStart(event)
}
const handleDragEnd = event => {
    const { target, x, y } = event
    target.classList.remove('dragging')
    emit('rearrange', {x, y})
}

const handleRemove = () => {
    emit('remove')
}
</script>

<template>
    <div
        draggable="true"
        class="widget"
        :class="`col-${cols}`"
        @dragstart="handleDragStart"
        @touchstart="handleTouchStart"
        @dragend="handleDragEnd"
        @touchend="handleDragEnd">

        <slot />

        <button
            class="close-icon"
            @click="handleRemove">
            &times;
        </button>
    </div>
</template>

<style scoped>
.widget {
    position: relative;
    height: 200px;
    border-radius: 10px;
    cursor: grab;
    overflow: hidden;
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
.widget.dragging {
    opacity: 0.5;
    border: 5px dashed #fff;
    animation: wiggle 250ms ease-in-out infinite alternate;
    cursor: grabbing;
}

.close-icon {
    position: absolute;
    top: 5px;
    right: 10px;
    font-size: 24px;
    width: 32px;
    height: 32px;
    padding: 5px;
    border: none;
    background-color: #0007;
    color: #fff;
    border-radius: 50%;
    font-family: sans-serif;
    line-height: 0;
    cursor: pointer;
}
.close-icon:hover {
    background-color: #a00;
}
@keyframes wiggle {
    from {
        transform: rotate(0.5deg);
    }
    to {
        transform: rotate(-0.5deg);
    }
}
</style>