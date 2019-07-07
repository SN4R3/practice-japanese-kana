<style scoped>
#textarea-contain {
  display: inline-block;
  width:80%;
}
.default {
  border-radius: 5px;
  border: none;
  padding: 5px;
}
#textarea-contain label.default {
  background: #f5cf2f;
  padding: 6.5px;
  margin-right: -3px;
  border-top-left-radius: 5px;
  border-bottom-left-radius: 5px;
  color: #676363;
}
label.admin {
  background:transparent;
  font-size:13px;
  display: block;
}
textarea.admin {
  width: 100%;
  height: 200px;
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
</style>
<template>
	<div id="textarea-contain" :class="sizes">
    <label :class="theme" v-show="name.length > 0">{{ name }}</label>
    <jodit-editor :class="theme" :placeholder="placeholder" v-model="value"></jodit-editor>
	</div>
</template>
<script>
  import 'jodit/build/jodit.min.css';
  import JoditVue from 'jodit-vue';
	export default {
    components: {
      'jodit-editor': JoditVue
    },
		props: {
      refer: { default:null},
      ph: { type:String },
      label: { type:String, default:false },
      val: { default: 'null' },
      theme: { type:String, default:"default" },
      required: {},
      col: { required: false, default: null },
      sizes: { default: ''}
    },
    data() {
      return {
        ref: this.refer,
        placeholder: this.ph,
        value: this.val,
        name: this.label
      }
    },
    watch: {
      val: function(newVal, oldVal) {
        this.value = newVal;
      }
    },
    methods: {
      validate() {
        let resp = {
          result: true,
          msg: ''
        };
        if(this.required && (!this.value || this.value === 'null')) {
          resp.result = false;
          resp.msg = this.name + ' is required';
        } else if (this.value) {
          if(this.value.length > 0 && this.col.length > this.value.length) {
            resp.result = false;
            resp.msg = this.name + ' must be at least '+ this.col.length + ' characters long';
          }
        }
        return resp;
      },
    }
	}
</script>