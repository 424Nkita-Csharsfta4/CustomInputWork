<template>
  <div class="autocomplete">
    <!-- Текстовое поле с двусторонней привязкой состояния -->
    <input v-model="state" placeholder="Ищи" @focus="handleFocus" @blur="handleBlur" />
    <!---@input="handleChange" - обработчик события изменения значения в текстовом поле, 
        вызывает метод handleChange из скрипта компонента -->
    <!----@focus="handleFocus" - обработчик события фокусировки на текстовом поле, 
          вызывает метод handleFocus из скрипта компонента-->
    <!--@blur="handleBlur" - обработчик события потери фокуса текстовым полем,
             вызывает метод handleBlur из скрипта компонента--->

    <Icons @click="handleIconClick" />

    <!-- Шаблон для отображения выпадающего списка -->
    <ul v-if="showList">
      <!-- Отображение каждого элемента списка -->
      <li v-for="item in filteredLinks" :key="item.value" @click="handleSelect(item)">
        <!-- Отображение значения элемента списка -->
        <div class="value">{{ item.value }}</div>
        <!-- Отображение ссылки элемента списка -->
        <span class="link">{{ item.link }}</span>
      </li>
    </ul>
  </div>
</template>


<script lang="ts" setup>
import Icons from './IconsItemView.vue'
import { computed, onMounted, ref } from 'vue'

const emit = defineEmits<{
  (eventName: "update:modelValue", value: string): void;
}>();

const props = defineProps<{
  modelValue: string;
}>();

// Определение типа элемента списка
interface LinkItem {
  value: string
  link: string
}

/** Создание реактивной переменной для хранения состояния компонента */
const state = computed({
  get() {
    return props.modelValue;
  },
  set(value) {
    emit("update:modelValue", value);
  }
})

// Создание реактивной переменной для хранения списка элементов
const links = ref<LinkItem[]>([])

// Создание реактивной переменной для отображения списка элементов
const showList = ref(false)

// Создание реактивной переменной для отображения отфильтрованного списка элементов
const filteredLinks = computed(() => links.value.filter(item => item.value.indexOf(state.value)))

// Функция для запроса подсказок
// const querySearch = (queryString: string, cb: (results: LinkItem[]) => void) => {
//   // Фильтрация списка элементов по строке запроса
//   const results = queryString ? links.value.filter(createFilter(queryString)) : links.value
//   // Вызов функции обратного вызова для возврата объектов подсказок
//   cb(results)
// }

// Функция для создания фильтра по строке запроса
// const createFilter = (queryString: string) => {
//   const lowerCaseQuery = queryString.toLowerCase()
//   return (link: LinkItem) => {
//     return link.value.toLowerCase().includes(lowerCaseQuery)
//   }
// }

// Изменение типа параметра на Event
// const handleChange = (event: Event) => {
//   state.value = (event.target as HTMLInputElement).value
//   querySearch(state.value, (results: LinkItem[]) => {
//     filteredLinks.value = results
//     showList.value = true
//   })
// }

// Изменение типа параметра на
const handleFocus = (event: FocusEvent) => {
  showList.value = true
}

// Изменение типа параметра на Event
const handleBlur = (event: Event) => {
  // showList.value = false
}

// Обработчик нажатия на иконку
const handleIconClick = () => {
  // querySearch(state.value, (results: LinkItem[]) => {
  //   filteredLinks.value = results
  //   showList.value = true
  // })
  state.value = "";
}

// Обработчик выбора элемента из списка
const handleSelect = (item: LinkItem) => {
  console.log(item);
  state.value = item.value
  // showList.value = false
}

// Вызов функции для получения списка элементов при монтировании компонента
onMounted(() => {
  links.value = [
    { value: 'Google', link: 'https://www.google.com' },
    { value: 'Facebook', link: 'https://www.facebook.com' },
    { value: 'Twitter', link: 'https://twitter.com' },
    { value: 'Instagram', link: 'https://www.instagram.com' },
    { value: 'LinkedIn', link: 'https://www.linkedin.com' },
  ]
})
</script>

<style scoped lang="less">
.autocomplete {
  position: relative;
  display: inline-block;
  width: 300px;
  display: flex;
}

input {
  width: 100%;
  height: 40px;
  padding: 10px;
  font-size: 16px;
  border: 1px solid #ccc;
  border-radius: 4px;
  outline: none;
}

ul {
  position: absolute;
  top: 100%;
  left: 0;
  z-index: 1;
  width: 100%;
  max-height: 200px;
  overflow-y: auto;
  margin: 0;
  padding: 0;
  list-style: none;
  background-color: #fff;
  border: 1px solid #ccc;
  border-radius: 4px;
  box-shadow: 0 2px 4px rgba(0, 0, 0, 0.2);
}

.value {
  display: block;
  font-size: 16px;
  font-weight: bold;
  margin-bottom: 5px;
}

.link {
  display: block;
  font-size: 14px;
  color: #888;
}

/* Добавляем стили для иконки */
.autocomplete>button {
  position: absolute;
  top: 0;
  right: 0;
  width: 40px;
  height: 40px;
  background-color: transparent;
  border: none;
  cursor: pointer;
  outline: none;
}
</style>