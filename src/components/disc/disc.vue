<template>
  	<transition name="slide">
    	<music-list :title="title" :bg-image="bgImage" :songs="songs"></music-list>
  	</transition>
</template>

<script type="text/ecmascript-6">
  	import MusicList from 'components/music-list/music-list'
	import {getSongList} from 'api/recommend'
	import {ERR_OK} from 'api/config'
	import {mapGetters} from 'vuex'
	import {createSong} from 'common/js/song'
    import {getValue} from 'common/js/util';

  	export default {
    	computed: {
	      	title() {
	        	return this.disc.dissname
	      	},
	      	bgImage() {
	        	return this.disc.imgurl
	      	},
	      	...mapGetters([
		        	'disc'
		    ])
	    },
    	data() {
      		return {
        		songs: []
      		}
    	},
    	created() {
      		this._getSongList()
    	},
   		methods: {
      	async _getSongList() {
            if (!this.disc.dissid) {
                this.$router.push('/recommend')
                return
            }
            let res = await getSongList(this.disc.dissid);
            let list = getValue(res,["cdlist","0","songlist"],[])
            this.songs = this._normalizeSongs(list)
        },
      	_normalizeSongs(list) {
        	let ret = []
	        list.forEach((musicData) => {
	          	if (musicData.songid && musicData.albummid) {
	            	ret.push(createSong(musicData))
	          	}
	        })
        	return ret
      	}
    },
    components: {
      	MusicList
    }
  }
</script>

<style scoped lang="less">
.slide-enter-active, .slide-leave-active {
  transition: all 0.3s;
}

.slide-enter, .slide-leave-to {
  transform: translate3d(100%, 0, 0);
}

</style>
