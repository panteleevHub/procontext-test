<template>
    <div class="app-list">
      <div class="app-list__caption">
        <button
          @click="onChevronClick(list.id, !list.isOpened)"
          :style="list.isOpened && {transform: 'rotate(0deg)'}"
          class="app-list__chevron"
          type="button"
        >
        </button>
        <input
          @change="onListChange(list.id, !list.isChecked)"
          :checked="list.isChecked"
          :class="!list.items.every(item => item.isChecked) && 'app-list__input--dot'"
          :id="'list' + list.id"
          class="visually-hidden app-list__input"
          type="checkbox"
        >
        <label :for="'list' + list.id">{{ list.name }}</label>
      </div>
      <ul v-if="list.isOpened" class="app-list__items">
        <li v-for="(item, itemId) in list.items" class="app-list__item">
          <div class="app-list__item-caption">
            <input
              @change="onItemChange(list.id, itemId, !item.isChecked)"
              :checked="item.isChecked"
              :id="'item' + list.id + itemId"
              class="visually-hidden" type="checkbox"
            >
            <label :for="'item' + list.id + itemId">{{ item.name }}</label>
          </div>
          <div class="app-list__inputs">
            <input
              @input="onCountChange(list.id, itemId, $event)"
              :value="item.count"
              class="app-list__count"
              type="number"
            >
            <div>
              <label
                :for="'color' + list.id + itemId"
                :style="{backgroundColor: item.color}"
                class="app-list__color"
              >
              </label>
              <input
                v-model="item.color"
                :id="'color' + list.id + itemId"
                class="visually-hidden"
                type="color"
              >
            </div>       
          </div>
        </li>
      </ul>
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

  emits: ['toggleList', 'toggleItem', 'chevronClick', 'changeCount'],

  methods: {
    onListChange(listId, bool) {
      this.$emit('toggleList', {listId, bool});
    },

    onItemChange(listId, itemId, bool) {
      this.$emit('toggleItem', {listId, itemId, bool})
    },

    onChevronClick(listId, bool) {
      this.$emit('chevronClick', {listId, bool});
    },

    onCountChange(listId, itemId, {target}) {
      this.$emit('changeCount', {listId, itemId, count: Number(target.value)});
    }
  }
}
</script>

<style scoped>
.app-list__caption {
  position: relative;
  padding-left: 30px;
}

.app-list__chevron {
  position: absolute;
  content: "";
  top: 0;
  left: 0;
  transform: rotate(-90deg);
  width: 20px;
  height: 18px;
  padding: 0;
  border: none;
  background-size: 20px 18px;
  background-image: url('../assets/down-chevron.svg');
  background-color: transparent;
  cursor: pointer;
}

.app-list__caption label,
.app-list__item-caption label {
  position: relative;
  padding-left: 30px;
}

.app-list__caption label::before,
.app-list__item-caption label::before {
  position: absolute;
  top: 0;
  left: 0;
  content: "";
  width: 18px;
  height: 18px;
  border: 1px solid black;
}

.app-list__caption .app-list__input:checked + label::after,
.app-list__item-caption input:checked + label::after {
  position: absolute;
  content: "";
  top: 0;
  left: 0;
  width: 18px;
  height: 18px;
  background-size: 18px 18px;
  background-image: url('./../assets/check-mark.svg');
}

.app-list__caption .app-list__input--dot:checked + label::after {
  top: 6px;
  left: 6px;
  width: 6px;
  height: 6px;
  background-image: none;
  background-color: black;
}

.app-list__items {
  display: flex;
  flex-direction: column;
  row-gap: 10px;
  padding: 15px 0;
  padding-bottom: 5px;
  padding-left: 60px;
}

.app-list__item {
  display: flex;
  justify-content: space-between;
}

.app-list__inputs {
  display: flex;
  column-gap: 10px;
}

.app-list__count {
  width: 50px;
  text-align: right;
  padding: 0;
  border: none;
}

.app-list__color {
  display: block;
  width: 20px;
  height: 20px;
  cursor: pointer;
}
</style>