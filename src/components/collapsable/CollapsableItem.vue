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
      ref="content"
      class="overflow-hidden transition-all duration-200"
      :class="{ 'opacity-0': !isOpen }"
      :style="heightStyles"
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
    const dynamicHeight = ref(0);
    const contentHeight = ref(0);
    const content = ref(null);
    const root = ref(null);
    const closeItems = inject<Function>('closeItems');
    const accordion = inject<Ref<boolean>>('accordion');

    const isOpen = computed(() => dynamicHeight.value !== 0);
    const heightStyles = computed(() => {
      const height = String(dynamicHeight.value) + 'px';
      return { maxHeight: height };
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
      const element = content.value as unknown as HTMLElement;
      contentHeight.value = element.scrollHeight;

      if (isOpen.value) {
        dynamicHeight.value = element.scrollHeight;
      }
    };

    onMounted(() => {
      window.addEventListener('resize', updateHeight);
      updateHeight();
    });

    onBeforeUnmount(() => {
      window.removeEventListener('resize', updateHeight);
    });

    return {
      toggle,
      heightStyles,
      isOpen,
      content,
      root,
    };
  },
});
</script>