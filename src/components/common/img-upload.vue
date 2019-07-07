<style scoped>
  #img-upload-container {
    padding:20px;
  }
  label {
    float:left;
    margin-right:5px
  }
  label.admin {
    background:transparent;
    font-size:13px;
    margin: 10px;
    font-weight:700;
  }
</style>
<template>
	<div id="img-upload-container">
    <label :class="theme" for="img-upload">
      {{ label }}
    </label>
    <span v-show="complete" style="color:green; font-size:14px">{{ valNoTimeStamp }} Uploaded <i class="fa fa-check"></i></span>
    <input type="file" :class="theme" accept="image/*" @change="processFile($event)"/>
    <img v-show="complete || value" :src="value" width="10%"/>
    <i v-show="value">{{ valNoTimeStamp }}</i>
	</div>
</template>
<script>

	export default {
		props: {
      refer: { default:null},
      label: { type:String, default: '' },
      theme: { type:String, default: 'default'},
      val: { type:String, default: null },
      path: { default: ''},
      imgname: { default:null }
    },
    data() {
      return {
        ref: this.refer,
        file: null,
        value: this.val,
        complete: false
      }
    },
    watch: {
      complete: function() {
        let self = this;
        setTimeout(() => { self.complete = false; }, 50000);
      },
      val: function(newVal) {
        this.value = newVal;
      },
    },
    computed: {
      valNoTimeStamp: function() {
        if(this.value) {
          return this.value.split('?')[0];
        }
        return '';
      }
    },
    methods: {
      processFile(e) {
        let self = this;
        let formData = new FormData();
        this.file = e.target.files[0];
        this.value = this.file.name;
        let headers = { 
          headers: { 'Content-Type': 'multipart/form-data' }
        };
        formData.append('image', this.file, this.value);
        formData.append('path', this.path);
        if(this.imgname) {
          formData.append('name', this.imgname);
        }
        axios.post('/api/admin/uploadImg', formData, headers).then(function(resp) {
          setTimeout(() => { 
            self.complete = true;
            self.value = resp.data +  '?timestamp=' + new Date().getTime();
          }, 1000);
        })
        .catch(function() {
        });
      },
      validate() {
        return { result: true, msg: 'Please upload image for ' +this.name };
      }
    }
	}
</script>