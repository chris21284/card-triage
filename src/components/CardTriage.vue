<template>
  <div class="card-triage">
    <div class="search">
      &#128203; Work of Christophe LAFFAILLE (21/03/2022)
      <div class="container-input">
        &#128269;
        <input type="text" v-model="inputSearch" placeholder="Search by patient name or arrhythmias"/>
      </div>
    </div>
    <div class="lists">
      <PresentationTest />
      <CardList :array="toDoList" @update="onUpdate" :id="idToDoList" name="to do" :filter="inputSearch"/>
      <CardList :array="doneList" @update="onUpdate" :id="idDoneList" name="done" :filter="inputSearch"/>
    </div>
  </div>
</template>

<script>
import CardList from "@/components/CardList";
import PresentationTest from "@/components/PresentationTest";
export default {
  name: 'CardTriage',
  components: {CardList, PresentationTest},
  props: {
    cards: {
      type: Array,
      default: () => []
    },
  },
  data() {
    return {
        inputSearch: '',
        idToDoList: 'to-do-list',
        idDoneList: 'done-list'
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
      this.$emit("update", {...change, newStatus: change.column == this.idToDoList ? 'REJECTED' : 'DONE'});
    },
    filterOnArray(array, filter) {
      for(let elem of array) {
        if(elem.toLowerCase().includes(filter)) {
          return true;
        }
      }
      return false;
    }
  },
  computed:{
    filteredList() {
      if(Array.isArray(this.cards)) {
        return this.cards.filter(card => card.patient_name.toLowerCase().includes(this.inputSearch.toLowerCase()) || this.filterOnArray(card.arrhythmias, this.inputSearch));
      }
      return [];
    },
    toDoList()  {
        return this.filteredList.filter(card => card.status == "PENDING" || card.status == "REJECTED");
    },
    doneList() {
        return this.filteredList.filter(card => card.status == "DONE");
    }
  },
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style>
  .card-triage {
    width: 100%;
    height: 100%;
    display: flex;
    flex-direction: column;
  }

  .card-triage .search {
    justify-content: flex-end;
    display: flex;
    border-bottom: 1px solid darkgrey;
    background-color: #e3e3e3;
    align-items: center;
    padding: 7px 10px;
    font-size: 17px;
    font-weight: bolder;
  }

  .card-triage .search .container-input {
    border-radius: 4px;
    padding-left: 4px;
    border: 1px solid #bcbcbc;
    background-color: white;
    margin-left: auto;
  }

  .card-triage .search .container-input:focus-within {
    border-color: #2fb1ff;
  }

  .card-triage .search input {
    border-radius: 4px;
    border: 0px;
    height: 26px;
    width: 230px;
    outline-color: transparent;
    outline-width: 0px;
  }

  .card-triage .lists {
    display: flex;
    padding-left: 20px;
    padding-right: 20px;
    gap: 10px;
    min-width: 900px;
    height: calc(100% - 47px);
    overflow: auto;
  }

  .card-triage .lists .card-list {
    width: 400px;
  }

  .card-triage .presentation {
    width: 500px;
    padding-right: 60px;
  }

  @media (max-width: 1200px) {
    .card-triage .presentation {
      display: none;
    }
  }
</style>
