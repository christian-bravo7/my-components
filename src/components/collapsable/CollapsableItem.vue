<template>
  <div 
    ref="root"
    class="shadow-md p-4 mb-4 last:mb-0" 
  >
    <header
      class="text-xl mb-2 cursor-pointer"
      @click="toggle"
    >
      <slot name="header" />
    </header>
    <div
      ref="collapsableContent"
      class="overflow-hidden transition-all duration-200"
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
  setup () {
    const collapsableContent = ref(null);
    const root = ref(null);

    const dynamicHeight = ref(0);
    const contentHeight = ref(0);

    const closeItems = inject<Function>('closeItems');
    const accordion = inject<Ref<boolean>>('accordion');

    const isOpen = computed(() => dynamicHeight.value !== 0);
    const dynamicStyles = computed(() => ({
      maxHeight: String(dynamicHeight.value) + 'px',
    }));

    onMounted(() => {
      window.addEventListener('resize', updateHeight);
      updateHeight();
    });

    onBeforeUnmount(() => {
      window.removeEventListener('resize', updateHeight);
    });

    const close = (): void => { dynamicHeight.value = 0; };
    const open = (): void => { dynamicHeight.value = contentHeight.value; };

    const toggle = (): void => {
      if (accordion!.value) {
        closeItems!(root.value);
      }

      if (!isOpen.value) {
        open();
      } else {
        close();
      }
    };

    const updateHeight = (): void => {
      contentHeight.value = (collapsableContent.value as unknown as HTMLElement).scrollHeight;

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
      close,
    };
  },
});
</script>