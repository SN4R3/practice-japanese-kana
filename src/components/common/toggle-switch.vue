<style scoped>

.switch {
  position: relative;
  display: inline-block;
  width: 60px;
  height: 34px;
}

.switch input {display:none;}

.slider {
  position: absolute;
  cursor: pointer;
  top: 0;
  left: 0;
  right: 0;
  bottom: 0;
  background-color: #ccc;
  -webkit-transition: .4s;
  transition: .4s;
}

.slider:before {
  position: absolute;
  content: "";
  height: 26px;
  width: 26px;
  left: 4px;
  bottom: 4px;
  background-color: white;
  -webkit-transition: .4s;
  transition: .4s;
}

input:checked + .slider {
  background-color: #2196F3;
}

input:focus + .slider {
  box-shadow: 0 0 1px #2196F3;
}

input:checked + .slider:before {
  -webkit-transform: translateX(26px);
  -ms-transform: translateX(26px);
  transform: translateX(26px);
}

/* Rounded sliders */
.slider.round {
  border-radius: 34px;
}

.slider.round:before {
  border-radius: 50%;
}
.default #label {
  background: #f5cf2f;
  padding: 6.5px;
  margin-right: -3px;
  border-top-left-radius: 5px;
  border-bottom-left-radius: 5px;
  color: #676363;
}
.admin #label {
  background:transparent;
  font-size:13px;
  font-weight: 700;
  display: block;
  padding-right: 50px;
}
</style>
<template>
	<div id="toggle-switch-contain" :class="theme">
    <span id="label">{{ name }}</span>
    <label class="switch">
      <input type="checkbox" v-model="value">
      <span class="slider" :class="switchstyle"></span>
    </label>
	</div>
</template>
<script>
	export default {
		props: {
      refer: { default:null},
      label: { type:String, required:true },
      val: { default: false},
      switchstyle: { default: ''},
      theme: { type:String, default: 'default'}
    },
    data() {
      return {
        ref: this.refer,
        name: this.label,
        value: this.val
      }
    },
    mounted() {
    },
    watch: {
      val: function(newVal, oldVal) {
        this.value = newVal;
      }
    },
    methods: {
      validate() {
        return  { result: this.value != undefined, msg: 'Something has gone wrong with input '+ this.name };
      },
    }
	}
</script>