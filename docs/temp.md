```html
<template>
	<div>
		<draggable v-model="myArray" :options="{group:'people'}" @start="drag=true" @end="drag=false">
			<div class="item-div" v-for="element in myArray" :key="element.id">{{element.name}}</div>
		</draggable>
		{{myArray}}
	</div>
</template>
<script>
	import draggable from 'vuedraggable'
	export default {
		name: '',
		data () {
			return {
				myArray:[
					{name:'a',id:1},
					{name:'b',id:2},
					{name:'c',id:3},
					{name:'d',id:4}
				]
			}
		},
		components: {
            		draggable,
        	}
	}
</script>

<style lang="less" scoped>
	.item-div {
		cursor: move;
		width: 80px;
		height: 30px;
		text-align: center;
		background: #ccc;
		margin: 10px;
	}
</style>
```

[github url](https://github.com/SortableJS/Vue.Draggable)
