<template>
	<div id="movie-detail">
		<md-theme :md-name="theme">
			<headerBack :title="movieDetail.title"></headerBack>
			<md-spinner :md-size="60" md-indeterminate v-show='spinnerFlag'></md-spinner>
			<md-card class="card-movie-detail" v-show='!spinnerFlag'>
			 <md-card-area md-inset>
			   <md-card-media md-ratio="9:16">
			     <img :src="movieDetail.src" alt="Coffee House">
			   </md-card-media>

			   <md-card-header>
			     <h2 class="md-title">{{movieDetail.title}}</h2>
			     <div class="md-subhead">
			       <p>{{movieDetail.type }}</p>
						 <p>时间：{{movieDetail.time}}</p>
			       <p>介绍：{{movieDetail.detail| json[1]}}</p>


			     </div>
			   </md-card-header>

			   <md-card-content>
			     {{movieDetail.summary}}
			   </md-card-content>
			 </md-card-area>

			 <md-card-content>
			   <h3 class="md-subheading">作者：{{movieDetail.author}}</h3>
			 </md-card-content>
			</md-card>
		</md-theme>
	</div>
</template>
<script>
import headerBack from "../common/header_back.vue"
import axios from 'axios';

export default {
	data(){
		return{
			movieId: this.$route.params.id,
			movieDetail: {},
			detail:{},
			spinnerFlag: true
		}
	},
	computed: {
		theme(){
	      return this.$store.getters.THEME_COLOR
	    }
	},
	mounted: function(){
		this.reqMovieDetail();
	},
	methods: {
		reqMovieDetail(){
		let detail = this.$store.getters.DETAIL_DATA;
		//axios.get(API_PROXY+'https://api.douban.com/v2/movie/subject/'+this.movieId)
		axios.get('./data/detail.json')
		        .then(function(res) {

		          this.movieDetail = {
							author : detail.author,
							id : detail.id,
							src : detail.src,
							time : detail.time,
							title : detail.title,
							type : detail.type,
							detail : res.data
							};
		          this.spinnerFlag = false;
		        }.bind(this))
		        .catch(function(error){
		          console.log(error);
		        });
		}
	},
	components: {
		headerBack,
		props: ['src'],
	}
}
</script>
<style lang="scss" scoped>
.card-movie-detail {
  margin-top: 72px;
  .md-subhead {
    .md-icon {
      $size: 16px;

      width: $size;
      min-width: $size;
      height: $size;
      min-height: $size;
      font-size: $size;
      line-height: $size;
    }

    span {
      vertical-align: middle;
    }
  }

  .card-reservation {
    margin-top: 8px;
    display: flex;
    align-items: center;
    justify-content: space-around;

    .md-icon {
      margin: 8px;
      color: rgba(#000, .54) !important;
    }

    .md-button {
      border-radius: 2px !important;
    }
  }
}
.md-spinner{
	margin-top: 72px;
}
</style>
