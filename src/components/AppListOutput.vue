<template>
  <div class="app-list">
    <div class="app-list__wrapper">
      <span>{{ list.name }}</span>
      <button
        v-if="list.isChecked"
        @click="handleSort(list.id, !list.isSorted)"
        class="app-list__sort"
        type="button"
      >
        {{list.isSorted ? 'Перемешать' : 'Сортировать'}}
      </button>
    </div>
    <ul v-if="list.isChecked && list.isSorted" class="app-list__items">
      <li
        v-for="(item, itemId) in list.items"
        :style="!item.isChecked && {display: 'none'}"
        class="app-list__item"
      >
        <button
          v-for="number of item.count"
          :style="{backgroundColor: item.color}"
          @click="onColorClick(list.id, itemId)"
        >
        </button>
      </li>
    </ul>
    <div
      v-if="list.isChecked && !list.isSorted"
      class="app-list__colors"
    >
      <button
        v-for="{color, itemId} in colors"
        :style="{backgroundColor: color}"
        @click="onColorClick(list.id, itemId)"
      >
      </button>
    </div>
  </div>
</template>

<script>
export default {
  props: {
    list: {
      type: Object,
      default: {},
      required: true,
    }
  },

  emits: ['removeColorUnit', 'sort'],

  methods: {
    onColorClick(listId, itemId) {
      this.$emit('removeColorUnit', {listId, itemId});
    },

    handleSort(listId, bool) {
      this.$emit('sort', {listId, bool});

      if (!bool) { this.shuffleColors() };
    },

    shuffleColors() {
      this.colors.forEach(color => {color.sort = Math.random()});
      this.colors.sort((a, b) => a.sort - b.sort);
    },
  },

  computed: {
    colors() {
      let colors = [];

      this.list.items.forEach((item, index) => {
        if (item.isChecked) {
          for (let i = 1; i < item.count; i++) {
            colors.push({color: item.color, itemId: index, sort: null});
          }
        }
      });

      return colors;
    }
  },

  watch: {
    colors() {
      this.shuffleColors();
    }
  }
}
</script>

<style scoped>
.app-list {
  border: 1px solid black;
  padding: 5px;
}

.app-list__wrapper {
  display: flex;
  justify-content: space-between;
}

.app-list__sort {
  background-color: dodgerblue;
  color: white;
  border-radius: 7px;
  border: none;
  padding: 3px 5px;
  cursor: pointer;
}

.app-list__items {
  display: flex;
  flex-direction: column;
  row-gap: 7px;
  padding: 7px 0 20px;
}

.app-list__items button,
.app-list__colors button {
  display: block;
  width: 12px;
  height: 12px;
  border: none;
  margin-right: 2px;
  margin-bottom: 2px;
  cursor: pointer;
}

.app-list__colors {
  display: flex;
  flex-wrap: wrap;
  padding-top: 7px;
  padding-bottom: 40px;
}

.app-list__item {
  display: flex;
  flex-wrap: wrap;
}
</style>