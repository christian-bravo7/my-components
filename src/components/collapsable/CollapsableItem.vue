<template>
  <div 
    ref="root"
    class="shadow-md p-4 mb-4 last:mb-0"
    @mouseover="openOnMouseover"
    @mouseout="closeOnMouseout"
  >
    <header
      class="text-xl mb-2 cursor-pointer"
      @click="toggle"
    >
      <slot name="header" />
    </header>
    <div
      ref="collapsableContent"
      class="overflow-hidden transition-all duration-200 ease-out"
      :class="{ 'opacity-0': !isOpen }"
      :style="dynamicStyles"
    >
      <slot name="default" />
    </div>
  </div>
</template>

<script lang="ts">
// eslint-disable-next-line no-unused-vars
import { defineComponent, inject, ref, onMounted, computed, Ref, onBeforeUnmount } from 'vue';

export default defineComponent({
  props: {
    open: {
      type: Boolean,
      default: false,
    },
  },
  setup (props) {
    const collapsableContent = ref(null);
    const root = ref(null);

    const dynamicHeight = ref(0);
    const contentHeight = ref(0);
    const initialOpened = ref(props.open);

    const closeItems = inject<Function>('closeItems');
    const accordion = inject<Ref<boolean>>('accordion');
    const openOnHover = inject<Ref<boolean>>('openOnHover');

    const isOpen = computed(() => dynamicHeight.value !== 0);
    const dynamicStyles = computed(() => {
      return !initialOpened.value
        ? {
            maxHeight: String(dynamicHeight.value) + 'px',
          }
        : {
            maxHeight: '100%',
          };
    });

    onMounted(() => {
      window.addEventListener('resize', updateHeight);
      updateHeight();
    });

    onBeforeUnmount(() => {
      window.removeEventListener('resize', updateHeight);
    });

    const closeContent = (): void => { 
      if (initialOpened.value) {
        initialOpened.value = false;
      }

      dynamicHeight.value = 0; 
    };
    const openContent = (): void => { dynamicHeight.value = contentHeight.value; };

    const toggle = (): void => {
      if (!openOnHover!.value) {
        if (accordion!.value) {
          closeItems!(root.value);
        }
  
        if (!isOpen.value) {
          openContent();
        } else {
          closeContent();
        }
      }
    };

    const openOnMouseover = (): void => {
      if (openOnHover!.value) {
        openContent();
      }
    };

    const closeOnMouseout = (): void => {
      if (openOnHover!.value) {
        closeContent();
      }
    };

    const updateHeight = (): void => {
      contentHeight.value = (collapsableContent.value as unknown as HTMLElement).scrollHeight;

      if (initialOpened.value) {
        dynamicHeight.value = contentHeight.value;
      }

      if (isOpen.value) {
        dynamicHeight.value = contentHeight.value;
      }
    };

    return {
      toggle,
      dynamicStyles,
      isOpen,
      collapsableContent,
      root,
      closeContent,
      openOnMouseover,
      closeOnMouseout,
      initialOpened,
    };
  },
});
</script>