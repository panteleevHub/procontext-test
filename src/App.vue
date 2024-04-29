<template>
  <div class="app">
    <ul class="app__input">
      <li v-for="list of lists">
        <AppListInput 
          @toggleList="onToggleList"
          @toggleItem="onToggleItem"
          @chevronClick="onChevronClick"
          @changeCount="onCountChange"
          :list="list"
        />
      </li>
    </ul>
    <ul class="app__output">
      <li v-for="list of lists">
        <AppListOutput
          @removeColorUnit="removeColorUnit"
          @sort="handleSort"
          :list="list"
        />
      </li>
    </ul>    
  </div>
</template>

<script>
import AppListInput from './components/AppListInput.vue';
import AppListOutput from './components/AppListOutput.vue';

export default {
  data() {
    return {
      lists: [
        {
          id: 0,
          name: 'List 1',
          isChecked: true,
          isSorted: true,
          isOpened: true,
          items: [
            {
              name: 'Item 1',
              color: '#ff0000',
              count: 10,
              isChecked: true,
            },
            {
              name: 'Item 2',
              color: '#ffff00',
              count: 10,
              isChecked: true,
            },
            {
              name: 'Item 3',
              color: '#008000',
              count: 10,
              isChecked: true,
            },
            {
              name: 'Item 4',
              color: '#0000ff',
              count: 10,
              isChecked: true,
            }
          ]
        },
        {
          id: 1,
          name: 'List 2',
          isChecked: false,
          isSorted: true,
          isOpened: false,
          items: [
            {
              name: 'Item 1',
              color: '#ff0000',
              count: 10,
              isChecked: false,
            },
            {
              name: 'Item 2',
              color: '#ffff00',
              count: 10,
              isChecked: false,
            },
            {
              name: 'Item 3',
              color: '#008000',
              count: 10,
              isChecked: false,
            },
            {
              name: 'Item 4',
              color: '#0000ff',
              count: 10,
              isChecked: false,
            }
          ]
        },
        {
          id: 2,
          name: 'List 3',
          isChecked: false,
          isSorted: true,
          isOpened: false,
          items: [
            {
              name: 'Item 1',
              color: '#ff0000',
              count: 10,
              isChecked: false,
            },
            {
              name: 'Item 2',
              color: '#ffff00',
              count: 10,
              isChecked: false,
            },
            {
              name: 'Item 3',
              color: '#008000',
              count: 10,
              isChecked: false,
            },
            {
              name: 'Item 4',
              color: '#0000ff',
              count: 10,
              isChecked: false,
            }
          ]
        },
        {
          id: 3,
          name: 'List 4',
          isChecked: false,
          isSorted: true,
          isOpened: false,
          items: [
            {
              name: 'Item 1',
              color: '#ff0000',
              count: 10,
              isChecked: false,
            },
            {
              name: 'Item 2',
              color: '#ffff00',
              count: 10,
              isChecked: false,
            },
            {
              name: 'Item 3',
              color: '#008000',
              count: 10,
              isChecked: false,
            },
            {
              name: 'Item 4',
              color: '#0000ff',
              count: 10,
              isChecked: false,
            }
          ]
        },
        {
          id: 4,
          name: 'List 5',
          isChecked: false,
          isSorted: true,
          isOpened: false,
          items: [
            {
              name: 'Item 1',
              color: '#ff0000',
              count: 10,
              isChecked: false,
            },
            {
              name: 'Item 2',
              color: '#ffff00',
              count: 10,
              isChecked: false,
            },
            {
              name: 'Item 3',
              color: '#008000',
              count: 10,
              isChecked: false,
            },
            {
              name: 'Item 4',
              color: '#0000ff',
              count: 10,
              isChecked: false,
            }
          ]
        }
      ]
    };
  },

  methods: {
    onToggleList({listId, bool}) {
      if (bool) {
        this.lists[listId].items.forEach(item => item.isChecked = true);
      } else {
        this.lists[listId].items.forEach(item => item.isChecked = false);
      }
      this.lists[listId].isChecked = bool;
    },

    onToggleItem({listId, itemId, bool}) {
      this.lists[listId].items[itemId].isChecked = bool;

      if (this.lists[listId].items.some(item => item.isChecked)) {
        this.lists[listId].isChecked = true;
      } else {
        this.lists[listId].isChecked = false;
      }
    },

    removeColorUnit({listId, itemId}) {
      this.lists[listId].items[itemId].count = this.lists[listId].items[itemId].count - 1;
    },

    handleSort({listId, bool}) {
      this.lists[listId].isSorted = bool;
    },

    onChevronClick({listId, bool}) {
      this.lists[listId].isOpened = bool;
    },
    
    onCountChange({listId, itemId, count}) {
      this.lists[listId].items[itemId].count = count;
    }
  },

  components: {
    AppListInput,
    AppListOutput,
  }
}
</script>

<style scoped>
.app {
  display: flex;
  justify-content: space-between;
  column-gap: 30px;
  width: 1000px;
  min-height: 600px;
  padding: 20px;
  margin: 70px;
  border: 2px solid black;
}

.app__input,
.app__output {
  width: 400px;
  border: 1px solid black;
}

.app__input {
  display: flex;
  flex-direction: column;
  row-gap: 10px;
  padding: 40px 15px;
  padding-right: 80px;
}

.app__output {
  display: flex;
  flex-direction: column;
  row-gap: 15px;
  padding: 20px;
}
</style>
