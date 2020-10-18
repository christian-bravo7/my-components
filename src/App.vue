<template>
  <img
    alt="Vue logo"
    src="./assets/logo.png"
  >
  <CollapsableSection :accordion="false">
    <CollapsableItem>
      <template #header>
        Hello world!
      </template>
      <template #default>
        Lorem ipsum dolor sit amet, consectetur adipisicing elit. Aut repellat maiores, qui totam officia dignissimos corporis ullam, odio quod sapiente adipisci eaque hic. Molestias sapiente obcaecati quam excepturi officia doloribus.
        Lorem ipsum dolor sit amet consectetur adipisicing elit. Maiores aut exercitationem, officiis accusantium ut error deserunt quod repellendus aliquid possimus et, pariatur ipsam voluptas accusamus, illo repellat sunt. Nobis, id.
      </template>
    </CollapsableItem>
    <CollapsableItem>
      <template #header>
        Hello world!
      </template>
      <template #default>
        Lorem ipsum dolor sit amet, consectetur adipisicing elit. Aut repellat maiores, qui totam officia dignissimos corporis ullam, odio quod sapiente adipisci eaque hic. Molestias sapiente obcaecati quam excepturi officia doloribus.
      </template>
    </CollapsableItem>
    <CollapsableItem>
      <template #header>
        Hello world!
      </template>
      <template #default>
        Lorem ipsum dolor sit amet, consectetur adipisicing elit. Aut repellat maiores, qui totam officia dignissimos corporis ullam, odio quod sapiente adipisci eaque hic. Molestias sapiente obcaecati quam excepturi officia doloribus.
      </template>
    </CollapsableItem>
  </CollapsableSection>
</template>

<script lang="ts">
import { defineComponent } from 'vue';
import CollapsableSection from './components/collapsable/CollapsableSection.vue';
import CollapsableItem from './components/collapsable/CollapsableItem.vue';

export default defineComponent({
  name: 'App',
  components: {
    CollapsableSection,
    CollapsableItem,
  },
});
</script>
