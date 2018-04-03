<style scoped lang='stylus'>
	.indexmusic{
		h3{
			font-weight: normal;
			font-size: 16px;height: 30px;line-height: 30px;
		}
		h3::before{
			content: "|";
			color: #c20c0c;font-size: 12px;
			background:#c20c0c;margin:0 5px;position: relative;top: -1px;
		}
		.albumList{
			.list{
				ul{
					display: flex;
					flex-wrap: wrap;
					li{
						width: 33.33%;
						div{
							width: 100%;
							img{
								width: 100%;display: block;
							}
						}
						p{
							height: 24px;line-height: 24px;
						}
					}
				}
			}
		}
	}
</style>
<template>
	<div class="indexmusic">
		<vui-swiper-x :swiperArr="swiperArr"></vui-swiper-x> 
		<div class="albumList">
			<h3>专辑列表</h3>
			<div class="list">
				<ul>
					<li v-for="(item,index) in albumList" @click="showAlbum(item,index)">
						<div>
							<img :src="item.albumImgSrc">
						</div>
						<p>{{item.albumTitle}}</p>
					</li>
				</ul>
			</div>
		</div>
		<div class="recommendList">
			<h3>热歌列表</h3>
		</div>
		<div>
			<transition name="slide">
				<album v-if="$store.state.album.isShowAlbum" :albumList="albumList[$store.state.album.index]"></album>
			</transition>
		</div>
		
	</div>
</template>
<script>
	import VuiSwiperX from "../../components/swiper.vue"
	
	import album from "../../components/album.vue"
	export default{
		data(){
			return{
				// 轮播图数据
				swiperArr:[{
					imgUrl:"http://p1.music.126.net/piXJCyIWqYGWEYy2AFnoBA==/19047939440081948.jpg",
				},{
					imgUrl:"http://p1.music.126.net/-pzOigsoJj3vZehOQqj6vA==/18801648837108169.jpg",
				},{
					imgUrl:"http://p1.music.126.net/JFBuZQCQJF9vCwsp3tnhzg==/18801648837108172.jpg",
				},{
					imgUrl:"http://p1.music.126.net/tN-BGO2bOm59xLvf7WOhCA==/18963277044745659.jpg",
				},{
					imgUrl:"http://p1.music.126.net/QlMwys9zc4Bg3Rm5qj8fsA==/19161189137713255.jpg",
				}],
				
			}
		},
		created(){
			this.$store.dispatch("GETALL")
		},
		computed:{
			albumList(){
				return this.$store.state.mapList.albumList
			}
		},
		methods:{
			showAlbum(item,index){

				// 显示 专辑页面
				this.$store.commit("SHOWORHIDEALBUM",{
					isShowAlbum : true,
					// 传入点击这张专辑
					album:item,
					// 这是第几张专辑
					index:index
				});
			}
		},
		components:{
			VuiSwiperX,album
		}
	}
</script>