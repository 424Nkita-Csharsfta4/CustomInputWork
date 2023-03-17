

<template>
  <div class="app">
    <AutocompleteInput v-model="searchText" :load-all="loadAll" :query-search-async="querySearchAsync"
      :handle-select="handleSelect" />
    <AutocompleteResults :links="filteredItems" v-if="searchText && showResults" @select-item="onSelectItem"
     :show-results="true" 
     :handle-select="handleSelect" 
    />
  </div>
</template> 

<script lang="ts">
import { ref, computed } from 'vue';
import AutocompleteInput from './InputView.vue';
import AutocompleteResults from './AutocompleteList.vue';
import type { LinkItem } from '../components/types/types';

export default {
  name: 'App',
  components: {
    AutocompleteInput,
    AutocompleteResults,
  },
  setup() {
    const items = ref<LinkItem[]>([
      { value: 'Google', link: 'https://www.google.com' },
      { value: 'Facebook', link: 'https://www.facebook.com' },
      { value: 'Twitter', link: 'https://twitter.com' },
      { value: 'Instagram', link: 'https://www.instagram.com' },
      { value: 'LinkedIn', link: 'https://www.linkedin.com' },
    ]);
    const searchText = ref('');
    const showResults = ref(false);

    const linksArray = computed(() => {
      return items.value.map((item) => {
        return { value: item.value, link: item.link };
      });
    });

    const filteredItems = computed(() => {
      if (!searchText.value) {
        return [];
      }
      return items.value.filter(
        (item) =>
          item.value.toLowerCase().indexOf(searchText.value.toLowerCase()) ===
          0
      );
    });

    const loadAll = () => {
      return items.value;
    };

    const querySearchAsync = (query: string, callback: (results: LinkItem[]) => void) => {
      const filtered = items.value.filter(
        (item) =>
          item.value.toLowerCase().indexOf(query.toLowerCase()) === 0
      );
      callback(filtered);
    };

    const handleSelect = (item: LinkItem) => {
      searchText.value = item.value;
      showResults.value = false;
    };

    const onSelectItem = (item: LinkItem) => {
      handleSelect(item);
    };

    return {
      searchText,
      showResults,
      filteredItems,
      loadAll,
      querySearchAsync,
      handleSelect,
      onSelectItem,
      linksArray,
    };
  },
};
</script>
