<template>
	<div class="shopcart">
		<div class="content" @click='toggleList'>
			<div class="content-left">
				<div class="logo-wrapper">
					<div class="logo" :class="{'highlight':totalCount>0}">
						<i class="icon-shopping_cart" :class="{'highlight':totalCount>0}">
							购物车
						</i>
					</div>
					<div class="num" v-show="totalCount>0" >
						{{totalCount}}
					</div>
				</div>
				<div class="price" :class="{'highlight':totalCount>0}">
					{{totalPrice}}元
				</div>
				<div class="desc">
					另需配送费{{deliveryPrice}}元
				</div>
			</div>
			<div class="content-right">
				<div class="pay" :class="payClass">
					{{payDes}}
				</div>
			</div>
		</div>
		<transition name="upward">
			<div class="shopcart-list" v-show="fold">
				<div class="list-header">
					<h1 class="title">购物车</h1>
					<span class="empty" @click="empty">清空</span>
				</div>
				<div class="list-content" ref = "listContent" >
					<ul>
						<li class="food" v-for="food in dataSource" v-if="food.count>0">
							<span class="name">{{food.food.name}}</span>
							<div class="price">
								<span>¥{{food.food.price*food.count}}</span>
							</div>
							<div class="cartcontrol-wrapper">
								<div  class="food-count"><span class="ride">x</span>{{food.count}}</div>
							</div>
						</li>
					</ul>
				</div>
			</div>
		</transition>
	</div>
</template>
<script type="text/javascript">
import cartcontrol from '../cartcontrol/cartcontrol'
import BScroll from 'better-scroll'
import Vue from 'vue'
	export default{
		props: {
			deliveryPrice: {
				type: Number,
				default: 0
			},
			minPrice: {
				type: Number,
				default: 0
			},
			dataSource:{
				type: Array,
				default () {
					return []
				}
			}
		},
		computed: {
			totalPrice() {
				let total = 0;
				for (var o of this.dataSource){
					total+=o.food.price*o.count
				}
				return total;
			},
			totalCount() {
				let count = 0;
				for (var o of this.dataSource){
					count+=o.count;
				}
				return count;
			},
			payDes() {
				if(this.totalPrice === 0){
					return `¥${this.minPrice}元起送`;
				}else if(this.totalPrice<this.minPrice){
					let diff = this.minPrice - this.totalPrice;
					return `还差¥${diff}元起送`;
				}else{
					return '去结算';
				}
			},
			payClass() {
				if(this.totalPrice<this.minPrice){
					return 'not-enough';
				} else {
					return 'enough';
				}
			},
		},
		created(){
			
		},
		data (){
			return {
				getcount:[],
				fold: false,
				show:false,
				items:[]
			}
		},
		components:{
			cartcontrol
		},
		methods:{
			toggleList() {
				if(this.totalCount>0) {
					this.fold===false;
					this.fold = !this.fold;
				}
				if(this.fold){
					this.$nextTick(() => {
						if(!this.scroll) {
							this.scroll = new BScroll(this.$refs.listContent,{
								click: true
							});
						} else {
							this.scroll.refresh();
						}
					})
				}
			},
			empty() {
				this.dataSource.forEach((food) => {
					food.count = 0;
				})
				this.fold = false;
			},
		},
	}
</script>
<style lang="stylus" rel="stylesheet/stylus">
@import "../../assets/stylus/mixin"
	.shopcart
		position: fixed
		left: 0
		bottom:0
		z-index: 50
		width: 100%
		height: 48px
		background: #000
		.content
			display: flex
			background: #141d27
			font-size : 0
			color: rgba(255,255,255,0.4)
			.content-left
				flex: 1
				.logo-wrapper
					display: inline-block
					position: relative
					vertical-align: top
					top: -10px
					margin: 0 12px
					padding: 6px
					width: 56px
					height: 56px
					box-sizing: border-box
					border-radius: 50%
					background: #141d27
					.logo
						width: 100%
						height: 100%
						vertical-align: top
						border-radius: 50%
						text-align: center
						background: #2b343c
						&.highlight
							background: rgb(0, 160, 220)
						.icon-shopping_cart
							line-height: 44px
							font-size: 24px
							color: #80858a
							&.highlight
								color: #fff
					.num
						position: absolute
						top: 0
						right: 0
						width: 24px
						height: 16px
						line-height: 16px
						text-align: center
						border-radius: 16px
						font-size: 9px
						font-weight: 700
						color: #fff
						background: rgb(240, 20, 20)
						box-shadow: 0 4px 8px 0 rgba(0,0,0,0.4)
				.price
					display: inline-block
					vertical-align: top
					margin-top: 12px 
					line-height: 24px
					padding-right: 12px
					box-sizing: border-box
					border-right: 1px solid rgba(255,255,255,0.1)
					font-size: 16px
					font-weight: 700
					&.highlight
						color: #fff
				.desc
					display: inline-block
					vertical-align: top
					line-height: 24px
					margin: 12px 0 0 12px
					font-size: 10px
			.content-right
				flex : 0 0 105px
				width: 105px
				.pay
					height: 48px
					line-height: 48px
					text-align: center
					font-size: 12px
					font-weight: 700
					&.not-enough
						background: #2b333b
					&.enough
						background: #00b43c
						color: #fff
		.shopcart-list
			position: absolute
			top: 0
			left:0
			z-index: -1
			width: 100%
			transform: translate3d(0, -100%,0)
			/*&.upward-enter-active, &.upward-leave-active
				transition:transform .5s*/
				/*transform: translate3d(0, -100%,0)*/
			/*&.upward-enter,&.upward-leave-active
				transform: translate3d(0,-200px,0)*/
			.list-header
				height: 40px
				line-height: 40px
				padding: 0 18px
				background: #f3f5f7
				border-bottom: 1px solid rgba(7,17,27,01)
				.title
					float: left
					font-size: 14px
					color: rgb(7,17,27)
				.empty
					float: right
					font-size: 12px
					color: rgb(0,160,220)
			.list-content
				overflow: hidden
				padding: 0 18px
				height: 217px
				background: #fff
				.food
					position: relative
					padding: 12px 0
					box-sizing: border-box
					border-1px(rgba(7, 17, 27, 0.1))
					.name
						line-height: 24px
						font-size: 14px
						color: rgb(7, 17, 27)
					.price
						position: absolute
						right: 90px
						bottom: 12px
						line-height: 24px
						font-size: 14px
						font-weight: 700
						color:  rgb(240, 20, 20)
					.cartcontrol-wrapper
						position: absolute
						right: 0
						bottom: 16px
						.food-count
							height: 14px
							line-height: 14px
							font-size: 14px
							color: #4d555d
							letter-spacing: 4px
							.ride
								font-size: 14px
						
						
				
</style>