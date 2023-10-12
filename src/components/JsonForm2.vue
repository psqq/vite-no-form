<template>
  <json-forms
    :data="data"
    :schema="schema"
    :uischema="uischema"
    :renderers="renderers"
    @change="onChange"
  />
</template>

<script lang="ts">
import { JsonForms } from '@jsonforms/vue';
import { vanillaRenderers } from '@jsonforms/vue-vanilla';
import { defineComponent } from 'vue';
import Group, { GroupEntry } from './Group.vue';

const renderers = [
  ...vanillaRenderers,
  GroupEntry
];

export default defineComponent({
  components: {
    JsonForms, Group
  },
  data() {
    return {
      renderers: Object.freeze(renderers),

      data: {
        number: 5,
      },
      schema: {
        properties: {
          number: {
            type: 'number',
          },
        },
      },
      uischema: {
        type: 'Group',
        elements: [
          {
            type: 'Control',
            scope: '#/properties/number',
          },
        ],
      },
    };
  },
  methods: {
    onChange(event: any) {
      this.data = event.data;
    },
  },
});
</script>