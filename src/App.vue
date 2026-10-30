<script setup lang="ts">
import BreakingNewsBubble from './components/BreakingNewsBubble.vue';
import { breakingNews } from './mocks/breakingNews.ts';

</script>

<template>
  <BreakingNewsBubble :news="breakingNews" />
</template>