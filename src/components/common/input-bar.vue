<style scoped>
#input-bar-contain {
  display: inline-block;
}
.default {
  border-radius: 5px;
  border: none;
  padding: 5px;
}
.hasIcon {
  border-top-right-radius: 0px;
  border-bottom-right-radius: 0px;
}
.hasLabel {
  border-top-left-radius: 0px;
  border-bottom-left-radius: 0px;
}
.fa {
  background-color:#f5cf2f;
  border:none;
  border-top-right-radius: 5px;
  border-bottom-right-radius: 5px;
  margin-left: -2px;
  border-left:1px solid #e4e2da;
  padding: 6.5px;
}
#input-bar-contain label.default {
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
  display:block;
  margin-bottom:0;
}
label.login {
  color:black;
}
input {
  padding:5px;
}
input.admin {
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
input.dark {
  width: 100%;
  background: #5a5254;
  border: 1px solid #3f3839;
  font-size: 22px;
}
input.light {
  width: 100%;
  font-size: 15px;
}
</style>
<template>
	<div :class="sizes" id="input-bar-contain">
    <label :class="theme" v-show="this.name.length > 0">
      {{ this.name }} 
      <i v-if="hidden" style="color:grey; font-size: 13px;">(Hidden)</i>
      <span v-if="required" style="color:red">*</span>
    </label>
    <input :type="inputType" 
      :hidden="hidden"
      ref="inputValue"
      :placeholder="this.placeholder"
      :style="'width:'+width"
      v-model="value"
      :class="[theme, inputclass, (faicon ? 'hasIcon': ''), (label ? 'hasLabel': '')]"
      class="padding"/>
    <i v-if="faicon" :class="'fa fa-'+faicon"></i>
	</div>
</template>
<script>
	export default {
		props: {
      refer: { default:null},
      inputType: { type:String, default:"text" },
      propname:{default:null},
      ph: { type:String },
      label: { type:String, default:false },
      val: {},
      width: { type:String, default:"unset" },
      theme: { type:String, default:"default" },
      sizes: { type:String },
      faicon: { default:false },
      action: { type:String },
      required: {},
      inputclass: {default: ''},
      hidden: {default:false},
      col: { required: false, default: null },
      specchars: { default:true }
    },
    data() {
      return {
        ref: this.refer,
        placeholder: this.ph,
        value: this.val,
        name: this.label
      }
    },
    mounted() {
      this.value = this.val;
    },
    watch: {
      val: function(newVal, oldVal) {
        this.value = newVal;
      },
      value: function(newVal, oldVal) {
        if(this.action) {
          this.$parent.$emit(this.action, newVal);
        }
      }
    },
	}
</script>