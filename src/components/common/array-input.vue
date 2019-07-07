<style scoped>
  .current-value .ele {
    border:1px solid black;
    border-radius: 5px;
    color:black;
    text-align: center;
    display:inline;
    position: relative;
    padding:10px;
    margin:10px 25px;
  }
  .fa-times:hover {
    font-weight: bolder;
    cursor: pointer;
  }
  /*Small devices (landscape phones, 576px and up)*/
  @media (min-width: 576px) {
  }
  /*Medium devices (tablets, 768px and up)*/
  @media (min-width: 768px) {
  }
  /*Large devices (desktops, 992px and up)*/
  @media (min-width: 992px) {
  }
  /*Extra large devices (large desktops, 1200px and up)*/
  @media (min-width: 1200px) { }
</style>
<template>
	<div class="col-12" id="array-input-contain">
    <input-bar
      ref="currVal"
      inputType="text"
      :ph="ph"
      theme="admin"
      sizes="col-9"
      width="80%"
      :label="label">
    </input-bar>
    <a class="btn btn-success col-2" style="color:white" @click="addItem()">Add</a>
    <div class="current-value row">
      <div class="ele" v-for="(ele, i) in items" :key="i">
        <i class="fa fa-times" style="color:red;positon:aboslute;top:8px;left:5px;" @click="deleteItem(ele)"></i>
        {{ele}}
      </div>
    </div>
  </div>
</template>

<script>
import InputBar from './input-bar';
export default {
  components: {
    'input-bar': InputBar,
  },
  props: {
    val: {},
    ph: {},
    label: {},
    refer: { default:null },
  },
  data() {
    return {
      ref: this.refer,
      items: this.val ? this.val.spilt(',') : [],
      value: this.val ? this.val : '',
    }
  },
  watch: {
    items: function(newVal, old) {
      this.value = newVal.join(',');
    },
    val: function(newVal, old) {
      this.items = newVal ? newVal.split(',') : [];
    }
  },
  methods: {
    addItem() {
      let ref = this.$refs.currVal.value;
      if(ref.length) {
        this.items.push(ref);
      }
      ref = null;
    },
    deleteItem(toDelete) {
      for(let i = 0; i < this.items.length; i++) {
        if(this.items[i] == toDelete) {
          this.items.splice(i, 1);
        }
      }

    }
  }
}
</script>
