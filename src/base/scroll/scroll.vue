<template>
	<div ref="wrapper">
		<slot></slot>
	</div>
</template>

<script>
	import BScroll from 'better-scroll';
	export default {
		props:{
			probeType: {
		        type: Number,
		        default: 1
	      	},
	      	click: {
	        	type: Boolean,
	        	default: true
	      	},
	      	listenScroll: {
	        	type: Boolean,
	        	default: false
	      	},
	      	data: {
	        	type: Array,
	        	default: null
	      	},
	      	pullup: {
	        	type: Boolean,
	        	default: false
	      	},
	      	beforeScroll: {
	        	type: Boolean,
	        	default: false
	      	},
	      	refreshDelay: {
	        	type: Number,
	        	default: 20
	      	}
		},
		mounted(){
			setTimeout(()=>{
				this._initScroll()
			},20)
		},
		methods:{
			_initScroll(){
				if (!this.$refs.wrapper) {
		          	return
		        }
				this.scroll=new BScroll(this.$refs.wrapper,{
					probeType: this.probeType,
					click:this.click,
				});
				
				if(this.listenScroll){
					let me=this;
					this.scroll.on("scroll",(res)=>{
						me.$emit("scroll",res)
					})
				}
				
        		if (this.pullup) {
          			this.scroll.on('scrollEnd', () => {
            			if (this.scroll.y <= (this.scroll.maxScrollY + 50)) {
              				this.$emit('scrollToEnd')
            			}
          			})
        		}
        		
        		if (this.beforeScroll) {
          			this.scroll.on('beforeScrollStart', () => {
            			this.$emit('beforeScroll')
          			})
        		}
			},
			enable() {
		        this.scroll && this.scroll.enable()
		    },
		    disable() {
		        this.scroll && this.scroll.disable()
		    },
		    refresh() {
		        this.scroll && this.scroll.refresh()
		    },
		    scrollTo() {
        		this.scroll && this.scroll.scrollTo.apply(this.scroll, arguments)
      		},
      		scrollToElement() {
        		this.scroll && this.scroll.scrollToElement.apply(this.scroll, arguments)
      		}
		},
		watch: {
	      	data() {
	        	setTimeout(() => {
	          		this.refresh()
	        	}, this.refreshDelay)
	      	}
	    }
	}
</script>

<style>
</style>