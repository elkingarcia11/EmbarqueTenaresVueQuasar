<template>
  <!-- Conditional rendering of toolbar components based on platform -->
  <MobileToolbarComponent v-if="$q.platform.is.mobile" />
  <AlternativeToolbarComponent v-else />
</template>

<script lang="ts">
import MobileToolbarComponent from './MobileToolbarComponent.vue';
import AlternativeToolbarComponent from './AlternativeToolbarComponent.vue';

export default {
  name: 'ToolbarWrapper',
  components: {
    MobileToolbarComponent, // Import of MobileToolbarComponent
    AlternativeToolbarComponent, // Import of AlternativeToolbarComponent
  },
};
</script>
