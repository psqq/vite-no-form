<template>
    <div>
        <div
            v-for="(element, index) in uischema.elements"
            v-bind:key="`${layout.path}-${index}`"
        >
            <dispatch-renderer
                v-bind:schema="layout.schema"
                v-bind:uischema="element"
                v-bind:path="layout.path"
                v-bind:enabled="layout.enabled"
                v-bind:renderers="layout.renderers"
                v-bind:cells="layout.cells"
            />
        </div>
    </div>
</template>

<script lang="ts">
import { uiTypeIs, JsonFormsRendererRegistryEntry, Layout, rankWith } from '@jsonforms/core';
import { computed, defineComponent } from 'vue';
import { DispatchRenderer, rendererProps, useJsonFormsLayout } from '@jsonforms/vue';

const GroupRenderer = defineComponent({
    name: 'GroupRenderer',
    components: {
        DispatchRenderer,
    },
    props: {
        ...rendererProps<Layout>(),
    },
    setup(props) {
        const jsonFormsLayout = useJsonFormsLayout(props);
        const uischema = computed(() => jsonFormsLayout.layout.value.uischema as Layout);
        return {
            layout: jsonFormsLayout.layout,
            uischema,
        };
    },
});

export default GroupRenderer;

export const GroupEntry: JsonFormsRendererRegistryEntry = {
    renderer: GroupRenderer,
    tester: rankWith(1000, uiTypeIs('Group')),
};
</script>
