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

<script>
    import SelectionArea from "@viselect/vue"

    export default {
        components: { SelectionArea },

        data()
        {
            return {
                selected: new Set(),
            }
        },

        methods:
        {
            extractIds(elements)
            {
                return elements.map((v) => v.getAttribute("data-key"))
                               .filter(Boolean)
                               .map(Number)
            },

            onStart({ event, selection })
            {
                if (!event?.ctrlKey && !event?.metaKey) {
                    selection.clearSelection()
                    this.selected.clear()
                }
            },

            onMove({ store: { changed: { added, removed } } }) {
                this.extractIds(added).forEach((id) => this.selected.add(id))
                this.extractIds(removed).forEach((id) => this.selected.delete(id))
            },

            range(to, offset = 0) {
                return new Array(to).fill(0).map((_, i) => offset + i)
            },
        },
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
