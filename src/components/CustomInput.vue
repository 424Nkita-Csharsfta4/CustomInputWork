<template>
  <div class="autocomplete">
    <input type="text" v-model="state" @input="onInput" placeholder="Ищи" />
    <IconsItemViewVue @click="handleIconClick" />
    <ul v-if="showResults">
      <li v-for="item in links" :key="item.link" @click="handleSelect(item)">
        <a :href="item.link">{{ item.value }}</a>
      </li>
    </ul>

  </div>
</template>

<script lang="ts">
import IconsItemViewVue from './IconsItemView.vue';
import { ref } from 'vue';

/**Интерфейс для объектов типа LinkItem */
interface LinkItem {
  value: string;
  link: string;
}

export default {
  setup() {

    /** Создание реактивной переменной для хранения состояния компонента */
    const state = ref<string>('');

    /**Создание реактивной переменной для отображения списка элементов */
    const showResults = ref<boolean>(false);

    /** Создание реактивной переменной для хранения списка элементов */
    const links = ref<LinkItem[]>([]);

    const loadAll = () => {
      return [
        { value: 'Google', link: 'https://www.google.com' },
        { value: 'Facebook', link: 'https://www.facebook.com' },
        { value: 'Twitter', link: 'https://twitter.com' },
        { value: 'Instagram', link: 'https://www.instagram.com' },
        { value: 'LinkedIn', link: 'https://www.linkedin.com' },
      ];
    };

    /**Эта функция принимает queryStringи функцию обратного cbвызова, которая принимает массив LinkItemобъектов.
     *Он фильтрует linksмассив для queryStringи передает отфильтрованные результаты функции обратного вызова. */
    const querySearchAsync = (queryString: string, cb: (arg: LinkItem[]) => void) => {
      const results = queryString
        ? links.value.filter(createFilter(queryString))
        : links.value;
    };
  /**Это вспомогательная функция, которая принимает queryStringи возвращает функцию queryString,
   *  которая соответствует valueсвойству LinkItemобъекта. */
    const createFilter = (queryString: string) => {
      return (item: LinkItem) => {
        return (
          item.value.toLowerCase().indexOf(queryString.toLowerCase()) === 0
        );
      };
    };


    /**Эта функция представляет собой обработчик событий,
     *который запускается при вводе в панель поиска автозаполнения. */
    const onInput = (event: Event) => {
      state.value = (event.target as HTMLInputElement).value;
      showResults.value = true;
      querySearchAsync(state.value, (results) => {
        links.value = results;
      });
    };
    
    /**Это функция, которая устанавливает свойство выбранного LinkItemобъекта и выводит его в логи Proxy */
    const handleSelect = (item: LinkItem) => {
      console.log(item);
      state.value = item.value;
      showResults.value = false;
    };

    // Обработчик нажатия на иконку
    const handleIconClick = () => {
      state.value = "";
    }

    /**linksпеременную в массив LinkItemобъектов,
     * представляющий набор ссылок,
     * которые будут отображаться в предложениях автозаполнения. */
    links.value = loadAll();

    return { state, showResults, links, onInput, handleSelect,handleIconClick };
  },
  components:{
    IconsItemViewVue
  }
};
</script>



<style  lang="less" scoped>
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
}
</style>