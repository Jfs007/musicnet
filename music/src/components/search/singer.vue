<template>
	<div class="singer">
		<ul class="search-list">
			<div class="loadMore" v-if='loadSearch&&loadings'>
				<loading v-if='loadings' :loadText='loadText' class='loading'></loading>
			</div>
			<li class="search-item search-item-native" v-for='list in searchLists'>
				<span class="item imgHeader">			
					<img :src='list.img1v1Url'/>
				</span>
				<span class="item name">
					<p>{{list.name}}</p>
				</span>
				<p class="cutline"></p>
			</li>
		</ul>
	</div>
</template>
<script type="text/javascript">
	import {
		search
	} from '../../api/getData.js';
	import {
		mapGetters,
		mapMutations,
		mapActions
	} from 'vuex';
	import loading from '../cmm/loading.vue';
	export default {
		data: function() {
			return {
				searchLists: [],
				isaddList: false,
				toast:false,
				key:'',
				loadings:true
			}
		},
		mounted: function() {

		},
		beforeRouteUpdate:function(to,from,next){
			
		},
		beforeRouteEnter: function(to, from, next) {
			next(vm => {
				vm.loadSearch == true &&( vm.searchLists = [] )
				vm.loadings = true;
				vm.key = vm.$route.query.key 
				vm.loadings&&vm.search();
			})
		},
		computed: {
			...mapGetters([
				'searchKey',
				'audio',
				'playList',
				'loadSearch'
			])
		},
		methods: {
			async search() {
				var res = await search(this.key, 100, 8, 0);
				this.searchLists = res.result.artists;
				this.loadings = false;
				this.searchStatus(false);
			},
			...mapMutations([
				'changeSong',
				'addSongList',
				'setAudio',
				'setLoading',
				'setCurrTime',
				'pause',
				'setIndex',
				'setBufferTime',
				'setPosCurrTime',
				'emptyList',
				'searchStatus'
			]),
			...mapActions({
				getUrl: 'getSong'
			}),
		},
		components: {
			loading
		}
	}
</script>
<style type="text/css" scoped>
	@import url("../../style/search.css");
	.singer .imgHeader{
		width:6rem;
		height: 6rem;
	}
	.singer .name{
		padding-left: 1.3rem;
	}
	.loadMore{
		height: 2.5rem;
		text-align: center;
		line-height: 2.5rem;
		color: #008000;
		box-sizing: content-box;
		padding-bottom: 8.6rem;
		position: relative;
	}
	.loadMore .loading{
		top:1rem;
	}
</style>