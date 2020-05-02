<template>
	<div @click="$emit('onClick')">
		<input v-show="inputStatus" :value="value" :id="id" :name="name" :type="type" :placeholder="placeholder" :disabled="disabled" 
			@change="onChange()" 
			@input="onInput" 
			@blur="onBlur"
			ref="cellValue"/>
		<div v-show="!inputStatus">{{formatNumber(value)}}</div>
	</div>
</template>

<script>
export default {
	name: "edit-table-cell",
	props: {
		value: { type: [String, Number], required: true },
		id: { type: String, required: false, default: () => Math.random().toString(36).substr(2, 9) },
		name: { type: String, required: false, default: '' },
		type: { type: String, required: false, default: 'text' },
		placeholder: { type: String, required: false, default: '' },
		disabled: { type: [Boolean, Number], required: false, default: false },
	   inputStatus :{ type: Boolean, default: false}
	},
	data(){
		return{}
	},
	created(){
		// console.log("Edittablecell", Math.random().toString(36).substr(2, 9));
	},

	methods:{
		formatNumber: function(val){
			if ( !isFinite(val) ){
				return val;
			}
			const valFloat = val.toString();
			const decPos = valFloat.indexOf("."); //may be 3rd number from last;
			let intPart = (decPos == -1)? valFloat:  valFloat.substr(0, decPos);
			let formatedNum = [];
			for(let ii in intPart){
				
				if (ii % 3 === 0 && ii != 0){
					formatedNum.push(',');
				}
				formatedNum.push(intPart[intPart.length - 1 - ii]);
			}
			formatedNum.reverse();

			return (decPos == -1)? formatedNum.join(""): formatedNum.join("") + valFloat.slice(decPos, valFloat.length);
		},
		onChange:function(event){
			this.$emit("input", this.$refs.cellValue.value);
		},
		onInput(){
			// console.log("oninput");
		},
		onBlur(){
			this.$emit("onBlur");
		}
	}
}
</script>

<style>

</style>