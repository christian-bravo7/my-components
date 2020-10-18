<script lang="ts">
import { defineComponent, toRefs, provide, h, ref } from 'vue';

const useCollapsableItemOptionsProvider = (props: any): void => {
  const { accordion, openOnHover } = toRefs(props);

  provide('accordion', accordion);
  provide('openOnHover', openOnHover);
};

const useCollapsableAccordion = (collapsableItemRefs: any): void => {
  const closeItems = (element: any): void => {
    collapsableItemRefs.forEach((collapsable: any) => {
      if (collapsable.$el !== element) {
        collapsable.closeContent();
      }
    });
  };

  provide('closeItems', closeItems);
};

export default defineComponent({
  props: {
    accordion: {
      type: Boolean,
      default: false,
    },
    openOnHover: {
      type: Boolean,
      default: false,
    },
  },
  setup (props) {
    const collapsableItemRefs = ref<Array<any>>([]);

    useCollapsableItemOptionsProvider(props);
    useCollapsableAccordion(collapsableItemRefs.value);

    const setCollapsableItemRef = (element: any): void => {
      collapsableItemRefs.value.push(element);
    };

    return {
      setCollapsableItemRef,
    };
  },
  render () {
    return h(
      'div', 
      {
        class: 'p-4 rounded-lg overflow-hidden',
      },
      [
        ...this.$slots.default!().map((item) => h(item, { ref: this.setCollapsableItemRef })),
      ]
    );
  },
});
</script>