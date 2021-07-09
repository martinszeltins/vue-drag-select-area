<template>
    <SelectionArea
        class="container"
        :options="{ selectables: '.selectable' }"
        :on-move="onMove"
        :on-start="onStart">

        <div
            v-for="id of range(400, 84)"
            :key="id"
            :data-key="id"
            class="selectable"
            :class="{ selected: selected.has(id) }"
        />
    </SelectionArea>
</template>

<script setup>
    import { ref } from 'vue'
    import SelectionArea from "@viselect/vue"

    const selected = ref(new Set())

    function extractIds(elements) {
        return elements.map((v) => v.getAttribute("data-key"))
                       .filter(Boolean)
                       .map(Number)
    }

    function onStart({ event, selection }) {
        if (!event?.ctrlKey && !event?.metaKey) {
            selection.clearSelection()
            selected.value.clear()
        }
    }

    function onMove({ store: { changed: { added, removed } } }) {
        extractIds(added).forEach((id) => selected.value.add(id))
        extractIds(removed).forEach((id) => selected.value.delete(id))
    }

    function range(to, offset = 0) {
        return new Array(to).fill(0).map((_, i) => offset + i)
    }
</script>

<style>
    html,
    body {
        width: 100%;
        height: 100%;
        display: flex;
        align-items: center;
        justify-content: center;
    }

    .container {
        padding: 0.5rem;
        display: grid;
        grid-gap: 0.5rem;
        grid-template-columns: repeat(20, 1fr);
        flex-wrap: wrap;
        width: 90vmin;
        height: 90vmin;
        background: #dbe9ff;
        border-radius: 0.5rem;
        user-select: none;
    }

    .container > div {
        background: #aaccff;
        border-radius: 0.25rem;
    }

    .container > div.selected {
        background: #5f9efc;
    }

    .selection-area {
        background: #4f90f22d;
        border: 1px solid #4f90f2;
    }
</style>
