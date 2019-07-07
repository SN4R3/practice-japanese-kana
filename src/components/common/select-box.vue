<style scoped>
div {
  display: inline-block;
}
select {
  width:70%;
  background-color:#ffff;
}
.default {
  border-radius: 5px;
  border: none;
  padding: 5px;
}
select.form-control {
  width:100%;
}
.col-lg-12 {
  padding-left:0;
}
.hasLabel {
  border-top-left-radius: 0px;
  border-bottom-left-radius: 0px;
}
label.default {
  background: #f5cf2f;
  padding: 6.5px;
  margin-right: -3px;
  border-top-left-radius: 5px;
  border-bottom-left-radius: 5px;
  color: #676363;
  max-width: 30%;
  font-size: 85%;
}
label.admin {
  background:transparent;
  font-size:13px;
  display:block;
  margin-bottom:0;
}
select.admin {
  width: 100%;
  height: 34px;
  margin: 10px;
  margin-right: 0px;
  padding: 6px 12px;
  font-size: 14px;
  line-height: 1.42857143;
  color: #555;
  background-color: #fff;
  background-image: none;
  border: 1px solid #ccc;
  border-radius: 4px;
}
select.multi {
  height:250px;
  font-size: 18px;
}
</style>
<template>
	<div :class="sizes">
    <label :class="theme" v-show="this.labelName.length > 0">
      {{ labelName }} 
      <span v-if="multi">
        <small style="color:red">(Hold CTRL + Click to select multiple)</small>
      </span>
      <span v-if="required" style="color:red">*</span>
    </label>
    <input v-if="filter" type="text" :placeholder="'Filter '+labelName" v-model="filterVal" style="width:100%;padding:7px;"/>
    <select 
      v-model="value" 
      :class="[theme, (label ? 'hasLabel': ''),multi ? 'multi' : '']" 
      :multiple="multi ? true : false"
    >
      <option :value="!multi ? null : []">{{nullName}}</option>
      <option v-for="(option, i) of optionsList"
        :key="option.val+'-'+i"
        :value="option.val">
        {{ option.name }}
      </option>
    </select>
	</div>
</template>
<script>
	export default {
		props: {
      refer: { default:null},
      label: { type:String, default:'', },
      propname:{default:null},
      val: { default:null },
      options: { },//Accepts arrays of Objets with 'val' & 'name' Properties
      theme: { type:String, default:'default' },
      sizes: { type:String },
      name: {},
      required: {default:false},
      action: { type:String, default:'' },
      nullName: { type:String, default:'Select an Option' },
      multi: { default: false},
      filter: { default:false }
    },
    data() {
      return {
        ref: this.refer,
        value: (!this.multi && !this.val) ? this.val : [],
        labelName: this.label,
        optionsList: this.options,
        optionsListCopy: this.options,
        filterVal: ''
      }
    },
    watch: {
      val: function(newVal, oldVal) {
        this.value = newVal;
        this.$parent.$emit(this.action, newVal);
      },
      value: function(newVal, oldVal) {
        this.$parent.$emit(this.action, newVal);
      },
      filterVal: function() {
        this.filterOptions();
      }
    },
    methods: {
      filterOptions() {
        this.optionsList = this.optionsListCopy;
        this.optionsList = this.optionsList.filter((option) => {
          if(option.name.toLowerCase().indexOf(this.filterVal.toLowerCase()) == -1) {
            return false;
          }
          return true;
        });
        //Select first option of filtered list
        this.value = this.optionsList.length ? this.optionsList[0].val : null;
      },
      validate() {
        let resp = {
          result: true,
          msg: ''
        };
        if(this.required && !this.value) {
          resp.result = false;
          resp.msg = this.label.replace(':','') + ' is required';
        }
        return resp;
      }
    }
	}
</script>