<template>
    <ul v-if="showResults">
        <li v-for="item in links" :key="item.link" @click="handleSelect(item)">
            <a :href="item.link">{{ item.value }}</a>
        </li>
    </ul>
</template>

  
<script lang="ts">
interface LinkItem {
    value: string;
    link: string;
}

export default {
    name: 'AutocompleteResults',
    props: {
        showResults: {
            type: Boolean,
            required: true
        },
        links: {
            type: Array as () => LinkItem[],
            required: true
        },
        handleSelect: {
            type: Function as unknown as () => (item: LinkItem) => void,
            required: true
        }
    }
};
</script>

  
<style lang="less" scoped>
ul {
    position: absolute;
    top: 100%;
    left: 0;
    width: 21.6%;
    margin: 0;
    padding: 0;
    list-style: none;
    background-color: #fff;
    border: 1px solid #ccc;
    border-top: none;
    border-radius: 0 0 4px 4px;
    overflow: hidden;

    li {
        padding: 8px 12px;
        border-bottom: 1px solid #ccc;

        &:hover {
            background-color: #f2f2f2;
        }

        a {
            color: #333;
            text-decoration: none;

            &:hover {
                text-decoration: underline;
            }
        }
    }
}
</style>
