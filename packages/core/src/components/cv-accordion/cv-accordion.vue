<template>
  <ul
    data-accordion
    :class="[
      `cv-accordion ${carbonPrefix}--accordion`,
      {
        [`${carbonPrefix}--accordion--${align}`]: align,
        [`${carbonPrefix}--accordion--${size}`]: size,
      },
    ]"
  >
    <slot></slot>
  </ul>
</template>

<script>
import { carbonPrefixMixin } from '../../mixins';

export default {
  name: 'CvAccordion',
  props: {
    align: { type: String, default: 'start', validator: val => ['start', 'end', ''].includes(val) },
    size: {
      type: String,
      default: '',
      validator: val => ['sm', 'md', 'lg', 'xl', ''].includes(val),
    },
  },
  mixins: [carbonPrefixMixin],
  created() {
    this.$on('cv:change', srcComponent => this.onCvChange(srcComponent));
  },
  computed: {
    state: {
      get() {
        const items = this.$children.filter(item => item.$_CvAccordionItem);
        return items.map(item => item.dataOpen);
      },
      set(val) {
        const items = this.$children.filter(item => item.$_CvAccordionItem);

        if (items.length > 0) {
          // loop through all items setting
          for (const i in items) {
            // making no attempt to verify array
            items[i].dataOpen = val[i] !== undefined ? !!val[i] : false;
          }
        }
      },
    },
  },
  methods: {
    onCvChange(srcComponent) {
      const index = this.$children.findIndex(item => item.uid === srcComponent.uid);
      this.$emit('change', { changedIndex: index, state: this.state });
    },
  },
};
</script>
