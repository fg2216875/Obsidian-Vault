```javascript
const app = Vue.createApp({
  data() {
    return {
      msg: '這是外層元件的 msg'
    }
  },
  methods:{
	setMsg(msg){
		this.msg = msg;
	}
  }
});

app.component('my-component', {
  template: `
    <div class="component">
      <div> 從 props 來的 parentMsg ==> {{ parentMsg }} </div>
      <div> 自己的 msg ==> {{ msg }} </div>
	  <button @@click="updateText">Update</button>
    </div>`,
  props: ["parentMsg"],
  data: () => {
    return {
      msg: '這是子元件的 msg',
	  parentNewMsg: this.parentMsg
    }
  },
  methods:{
	updateText(){
		//'update' =>事件名稱  value =>this.msg是指子層的！
		this.$emit('update', this.msg); 
	}
  }
});

app.mount('#app');
```

