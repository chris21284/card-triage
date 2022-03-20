<template>
  <div class="card-list">
    <h1>{{ name }}</h1>
    <draggable
        :id="id + '-draggable'"
        :list="array"
        item-key="id"
        class="list"
        @change="onChange"
        group="card">
      <template #item="{ element }">
        <CardElement :element="element" :filter="filter"/>
      </template>
    </draggable>
  </div>
</template>

<script>
import CardElement from "@/components/Card";
import draggable from 'vuedraggable';

export default {
  name: 'CardList',
  components: {CardElement, draggable},
  props: {
    array: {
      type: Array,
      default: () => []
    },
    id : {
      type: String,
      default: ''
    },
    name: {
      type: String,
      default: ''
    },
    filter: {
      type: String,
      default: ''
    }
  },
  methods: {
    onChange(change) {
      let newChange;
      if(change.added !== undefined) {
        if(change.added.newIndex != 0) {
          newChange = {...change, column: this.id, idPreviousElement: this.array[change.added.newIndex - 1].id};
        }
        else {
          newChange = {...change, column: this.id};
        }
      }
      if(change.moved !== undefined) {
        if(change.moved.newIndex != 0) {
          newChange = {...change, column: this.id, idPreviousElement: this.array[change.moved.newIndex - 1].id};
        }
      }
      if(newChange !== undefined) {
        this.$emit('update', newChange);
      }
    }
  }
}
</script>

<style>
  .card-list {
    margin-left: auto;
    margin-right: auto;
    background-color: #b9bcb6;
    /*border: 1px solid blue;*/
    width: 100%;
    height: 850px;
  }
  .card-list .list {
    height: calc(100% - 65px);
  }

  .card-list h1 {
    top: 0px;
    position: sticky;
    margin-top: 10px;
    margin-bottom: 20px;
    background-color: #b9bcb6;
    z-index: 2;
  }
</style>
