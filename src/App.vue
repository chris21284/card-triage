<template>
  <CardTriage :cards="cards" @update="onUpdate"/>
</template>

<script>
import CardTriage from './components/CardTriage.vue'

export default {
  name: 'App',
  components: {
    CardTriage
  },
  data() {
    return {
      cards: {
        type: Array,
        default: () => []
      },
    }
  },
  methods: {
    indexElementWithId(array, id) {
      return array.findIndex((element) => element.id == id);
    },
    arrayMove(array, oldIndex, newIndex) {
      if (newIndex >= array.length) {
        var k = newIndex - array.length + 1;
        while (k--) {
          array.push(undefined);
        }
      }
      array.splice(newIndex, 0, array.splice(oldIndex, 1)[0]);
      //return arr; // for testing
    },
    onUpdate(change) {
      if(change.added != undefined) {
        if(change.idPreviousElement !== undefined) {
          let newIndex = this.indexElementWithId(this.cards, change.idPreviousElement) + 1;
          let oldIndex = this.indexElementWithId(this.cards, change.added.element.id);
          this.arrayMove(this.cards, oldIndex, newIndex);
        }
        this.cards = this.cards.map(card => ( card.id === change.added.element.id ? {...card, status: change.newStatus} : card));
      }
      else if(change.moved != undefined) {
        if(change.idPreviousElement !== undefined) {
          let newIndex = this.indexElementWithId(this.cards, change.idPreviousElement) + 1;
          let oldIndex = this.indexElementWithId(this.cards, change.moved.element.id);
          this.arrayMove(this.cards, oldIndex, newIndex);
        }
        this.cards = this.cards.map(card => ( card.id === change.moved.element.id ? {...card, status: change.newStatus} : card));
      }


    }
  },
  mounted() {
    fetch('http://localhost:3000/cards', {
      method: 'get',
      headers: {
        'content-type': 'application/json'
      }
    }).then(res => {
      // a non-200 response code
      if (!res.ok) {
        // create error instance with HTTP status text
        const error = new Error(res.statusText);
        error.json = res.json();
        throw error;
      }
      return res.json();
    })
        .then(json => {
          // set the response data
          this.cards = [...json];
        })
  }
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  width:100vw;
  height: 100%;
  text-align: center;
  color: #2c3e50;
  background-color: #f4f4f4;
}

html, body {
  width: 100%;
  height: 100%;
}
</style>
