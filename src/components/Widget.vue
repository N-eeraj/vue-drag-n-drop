<script setup>
const emit = defineEmits(['rearrange'])

const handleDragStart = ({target}) => {
    target.classList.add('dragging')
}
const handleTouchStart = event => {
    event.preventDefault()
    handleDragStart(event)
}
const handleDragEnd = event => {
    const { target, x, y} = event
    target.classList.remove('dragging')
    emit('rearrange', {x, y})
}
</script>

<template>
    <div
        draggable="true"
        class="widget"
        @dragstart="handleDragStart"
        @touchstart="handleTouchStart"
        @dragend="handleDragEnd"
        @touchend="handleDragEnd">
        <slot />
    </div>
</template>

<style scoped>
.widget {
    height: 200px;
    padding: 5px;
    border-radius: 10px;
    cursor: grab;
}
.widget.dragging {
    opacity: 0.5;
    border: 5px dashed #fff;
    animation: wiggle 250ms ease-in-out infinite alternate;
    cursor: grabbing;
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