<template>
  <div class="card" :style="inlineStyle">
    <div class="title">
      &#128100; <div :class="filteredElement(element.patient_name, filter)">{{element.patient_name}}</div>
      <div class="date">&#128197; {{date(element.created_date)}}</div>
    </div>
    <div class="arrhythmia">
      Arrhythmias :
      <ul>
        <li :class="filteredElement(arrhythmia, filter)" v-for="(arrhythmia, index) in element.arrhythmias" :key="index">{{arrhythmia}}</li>
      </ul>
    </div>
  </div>
</template>

<script>
export default {
  name: 'CardElement',
  components: {},
  props: {
    element: {type: Object},
    filter: {
      type: String,
      default: ''
    }
  },
  computed: {
    inlineStyle() {
      // generate a number between -2 and 2
      return {
        transform: 'rotate(' + ((Math.random() * 4) - 2) + 'deg)'
      };
    }
  },
  methods: {
    date(createdDate) {
      const newDate = new Date(Date.parse(createdDate));
      return newDate.toLocaleDateString();
    },
    filteredElement(value, filter) {
      let classes = [];
      if(filter.length > 0 && value.toLowerCase().includes(filter)) {
        classes.push("filtered");
      }
      return classes;
    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style>

.card {
  font-family: 'Reenie Beanie';
  font-size: 22px;
  text-align: center;
  padding-top: 16px;
  padding-right: 10px;
  padding-left: 10px;
  margin: 15px;
  background-color: #ffffa5;
  box-shadow: 2px 4px 6px #444;
  /*transform: rotate(2deg);
  border-bottom-right-radius: 60px 10px;*/
}

.card .filtered {
  text-decoration: underline;
  font-weight: bold;
}

.card .title{
  font-size: 23px;
  display: flex;
  text-transform: capitalize;
}

.card .title .date {
  margin-left: auto;
}

.card .arrhythmia {
  text-align: left;
  padding-top: 15px;
}

.card .arrhythmia ul {
  margin: 0;
  padding-bottom: 4px;
  padding-left: 14px;
}

.card .arrhythmia ul li {
  text-align: left;
  display: inline-block;
  font-size: 18px;
}

.card .arrhythmia ul li:not(:last-child):after {
  content: ',\00a0';
  text-decoration: unset;
}
</style>
