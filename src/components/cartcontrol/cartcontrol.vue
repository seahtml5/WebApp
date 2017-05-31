<template>
	<div class="cartcontrol">
		<div class="cart-decrease icon-remove_circle_outline" v-show="info.count>0" @click="decreaseCart" transition="move">
			<span class="inner icon-remove_circle_outline">-</span>
		</div>
		<div class="cart-count" v-show="info.count>0">
			{{info.count}}
		</div>
		<div class="cart-add icon-add_circle" @click="addCart">
			+
		</div>
	</div>
</template>
<script type="text/javascript">
	import Vue from 'vue'
	export default {
		props:{
			food:{
				type:Object
			}
		},
		methods: {
			addCart(event) {
				if(!event._constructed){
					return;
				}
				this.info.count++;
				this.$emit('changeorder',this.info);
			},
			decreaseCart (event){
				if(!event._constructed){
					return;
				}
				this.info.count--;
				this.$emit('changeorder',this.info);
			}
		},
		data (){
			return {
				info: {
					food: this.food,
					count: 0,
				}
			}
		}
	}
</script>
<style lang="stylus" rel="stylesheet/stylus">
	.cartcontrol
		font-size: 0
		.cart-decrease
			display: inline-block
			padding: 6px
			transition: all 0.4s linear
			&.move-transition
				opacity: 1
				transform: translate3D(0, 0, 0)
			.inner
				line-height: 24px
				font-size: 24px
				color: rgb(0, 160, 220)
			&.move-enter, &.move-leave
				opacity: 0
				transform: translate3D(24px,0,0)
		.cart-count
			display: inline-block
			vertical-align: top
			width: 12px
			padding-top: 6px
			line-height: 24px
			text-align: center
			font-size: 10px
			color: rgb(147, 153, 159)
		.cart-add
			display: inline-block
			padding: 6px
			line-height: 24px
			font-size: 24px
			color: rgb(0, 160, 220)
			
</style>