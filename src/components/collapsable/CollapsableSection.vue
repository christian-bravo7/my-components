<script lang="ts">
import { defineComponent, toRefs, provide, h } from 'vue';

const useCollapseItemProvider = (props: any): void => {
  const { accordion, openOnHover } = toRefs(props);

  provide('accordion', accordion);
  provide('openOnHover', openOnHover);
};

const useCollapsableAccordion = (): any => {
  const collapsableItemRefs: any = [];

  const setCollapsableItemRef = (el: any): void => {
    collapsableItemRefs.push(el);
  };

  const closeItems = (element: any): void => {
    collapsableItemRefs.forEach((collapsable: any) => {
      if (collapsable.$el !== element) {
        collapsable.close();
      }
    });
  };

  provide('closeItems', closeItems);

  return { setCollapsableItemRef };
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
    useCollapseItemProvider(props);

    const { setCollapsableItemRef } = useCollapsableAccordion();

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