<template>
	<table class="edit-table">
		<thead>
			<slot name="columns">
			<th v-for="column in columns" :key="column.text">
				{{column.text}}
			</th>
			</slot>
			<!-- <tr>
			<th v-for="(col, ii) in columns" :key="ii">{{ col.text }}</th>
			</tr> -->
		</thead>
	<tbody>
		<tr v-for="(row, rowIndex) in rows" :key="rowIndex">
			<td v-for="column in columns" :key="column.text">
				<!-- <input type="text" :value="row[column.field]" /> -->
				<edit-table-cell v-model="row[column.field]" 
					@input="cellInput()"
					@onClick="onActiveCell(rowIndex, column.field)" 
					@onBlur="onBlur(rowIndex, column.field)"
					:inputStatus="isFocused(rowIndex, column.field)" > </edit-table-cell>
			</td>
		</tr>
	</tbody>
	</table>
</template>

<script>
import EditTableCell from '@/components/EditTableCell';
export default {
	name: 'EditTable',
	components:{
		EditTableCell
	},
	props: {
		columns: Array,
		rows: Array
	},
	data(){
		return{
			activeRow: '',
			activeCol: ''
		}
		
	},
	created(){
		let iii = 10;
		console.log("created");
	},
	methods:{
		cellInput(val){
			// console.log("cell input", val);
			this.$emit("changedTable");
		},
		onActiveCell(rowIndex, colField){
			console.log("onActive", rowIndex, colField);
			this.activeRow = rowIndex;
			this.activeCol = colField;
		},
		onBlur(row, col){
			this.activeRow = '';
			this.activeCol = '';
		},
		isFocused(row, col){
			
			if (this.activeRow === row && this.activeCol === col){
				return true;
			}
			return false;
		}
	}
}
</script>

<style scoped>

</style>
