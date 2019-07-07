<style scoped>

.default label {
  background: #f5cf2f;
  padding: 6.5px;
  margin-right: -3px;
  border-top-left-radius: 5px;
  border-bottom-left-radius: 5px;
  color: #676363;
}
.admin label {
  background:transparent;
  font-size:13px;
  font-weight: 700;
  padding-right: 50px;
}
</style>
<template>
	<div id="check-box-contain" :class="theme">
    <label>
      {{ name }}
      <input type="checkbox" v-model="checked" @change="changed">
    </label>
	</div>
</template>
<script>
	export default {
		props: {
      refer: {},
      label: { type:String, required:true },
      val: { required:true },
      theme: { type:String, default: 'default' }
    },
    data() {
      return {
        ref: this.refer,
        name: this.label,
        checked: this.val
      }
    },
    watch: {
      val: function(newVal, oldVal) {
        this.checked = newVal;
      }
    },
    methods: {
      validate() {
        return  { result: this.checked != undefined, msg: 'Something has gone wrong with input '+ this.name };
      },
    }
	}
</script>