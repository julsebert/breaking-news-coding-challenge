<script setup lang="ts">
  import type { NewsItem } from '../types/NewsItem';

  defineProps<{
    news: readonly NewsItem[];
  }>();
</script>

<template>
  <ul class="news-menu">
    <li
      v-for="(item, index) in news"
      :key="`${index}`"
    >
      <a
        :href="item.link"
        target="_blank"
        rel="noopener noreferrer"
      >
        {{ item.title }}
      </a>
    </li>
  </ul>
</template>

<style scoped>
  .news-menu {
    margin: 0;
    padding: 0;
    max-height: calc(100vh - 16px);
    list-style: none;
    border: 1px solid #3b3b3c;
    overflow-y: auto;
  }

  .news-menu a {
    display: block;
    padding: 4px;
  }

  .news-menu a:hover {
    background-color: #dff5f5;
  }
</style>
