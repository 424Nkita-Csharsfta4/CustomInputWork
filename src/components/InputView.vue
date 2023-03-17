<template>
    <div class="autocomplete">
        <input type="text" v-model="state" @input="onInput" placeholder="Ищи" />
        <IconsItemViewVue @click="handleIconClick" />
    </div>
</template>
  
<script lang="ts">
import IconsItemViewVue from './IconsItemView.vue';
import { ref } from 'vue';

export default {
    name: 'AutocompleteInput',
    props: {
        loadAll: {
            type: Function,
            required: true
        },
        querySearchAsync: {
            type: Function,
            required: true
        },
        handleSelect: {
            type: Function,
            default: () => []
        }
    },
    setup(props) {
        const state = ref<string>('');
        const showResults = ref<boolean>(false);
        const links = ref(props.loadAll());

        const onInput = (event: Event) => {
            state.value = (event.target as HTMLInputElement).value;
            showResults.value = true;
            props.querySearchAsync(state.value, (results: any) => {
                links.value = results;
            });
        };

        const handleIconClick = () => {
            state.value = "";
        }

        return { state, showResults, links, onInput, handleIconClick };
    },
    components: {
        IconsItemViewVue
    }
};
</script>
  
<style lang="less" scoped>
.autocomplete {
    position: relative;

    input {
        width: 20%;
        font-size: 16px;
        padding: 8px 12px;
        border: 1px solid #ccc;
        border-radius: 4px;
        outline: none;
    }
}
</style>
