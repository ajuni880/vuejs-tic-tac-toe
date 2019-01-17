<template>
    <div 
		class="square"  
		:class="'square-'+index" 
		:index="index" 
		@click="handleClick"
	>	
		{{ mark }}
	</div>
</template>

<script lang="ts">
import Vue from 'vue';
import { Component, Prop } from 'vue-property-decorator';

@Component
export default class Square extends Vue {
	@Prop() private index!: number;
	@Prop() private mark!: string;
	@Prop() private onClick: any;

	public handleClick() {
		this.$emit('mark', this.index);
	}
}
</script>

<style scoped>
	.square {
		padding: 40px;
		background: transparent;
		border: 2px solid #fff;
		color: aliceblue;
		height: 80px;
		display: flex;
		justify-content: center;
		align-items: center;
		font-size: 34px;
		cursor: pointer;
	}
	.square-0 {
		border-top: 0;
		border-left: 0;
	}
	.square-1 {
		border-top: 0;
	}
	.square-2 {
		border-right: 0;
		border-top: 0;
	}
	.square-3 {
		border-left: 0;
	}
	.square-5 {
		border-right: 0;
	}
	.square-6 {
		border-left: 0;
		border-bottom: 0;
	}
	.square-7 {
		border-bottom: 0;
	}
	.square-8 {
		border-bottom: 0;
		border-right: 0;
	}

</style>
