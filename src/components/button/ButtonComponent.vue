<template>
  <button
    ref="buttonRef"
    class="relative overflow-hidden bg-white p-4 rounded-2xl text-black border border-purple-800 hover:bg-purple-100 transition-colors duration-200"
    @click="onClick"
  >
    <span
      ref="rippleRef"
      class="ripple bg-purple-500"
      :style="dynamicStyles"
    />
    Hello world!
  </button>
</template>

<script lang="ts">
import { defineComponent, reactive, ref } from 'vue';

export default defineComponent({
  setup () {
    const buttonRef = ref(null);
    const rippleRef = ref(null);
    const activeRipple = ref(false);
    const dynamicStyles = reactive({
      left: 'initial',
      top: 'initial',
      transform: 'scale(0)',
      opacity: '1',
    });

    const onClick = (e: any): void => {
      if (!activeRipple.value) {
        activeRipple.value = true;

        const { offsetX: mouseX, offsetY: mouseY } = e;
        const { offsetHeight: buttonHeight, offsetWidth: buttonWidth } = e.target;
  
        const longestSide = Math.max(buttonHeight, buttonWidth);

        dynamicStyles.left = String(mouseX) + 'px';
        dynamicStyles.top = String(mouseY) + 'px';
        dynamicStyles.transform = `scale(${String(longestSide * 2)})`; 
        dynamicStyles.opacity = '0';

        (rippleRef.value! as HTMLElement).ontransitionend = (e): void => {
          console.log(e);
          console.log('first');
  
          (rippleRef.value! as HTMLElement).ontransitionend = (e): void => {
            console.log(e);
            console.log('second');

            (rippleRef.value! as HTMLElement).ontransitionend = (e): void => {
              console.log(e);
              console.log('third');

              (rippleRef.value! as HTMLElement).ontransitionend = (e): void => {
                console.log(e);
                console.log('fourth');

              };
            };
          };
        };
      } else {
        dynamicStyles.transform = `scale(${String(0)})`;
        dynamicStyles.left = 'initial';
        dynamicStyles.top = 'initial';
        dynamicStyles.opacity = '1';

        activeRipple.value = false;
      }
    };

    return {
      onClick,
      buttonRef,
      rippleRef,
      dynamicStyles,
    };
  },
});
</script>

<style lang="scss" scoped>
.ripple {
  position: absolute;
  width: 1px;
  height: 1px;
  transform: scale(0);
  transform-origin: center;
  transition: transform 4s, opacity 4s;
  transition-timing-function: ease-out;
  opacity: 0.5;
  border-radius: 50%;
}
</style>
