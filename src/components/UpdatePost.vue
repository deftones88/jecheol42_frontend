<template>
		<div class="l_wrapper">
			<div class="content">
				<div class="background">
					<form class="createPostArea">
						<div class="createInfo">게시글 수정</div>
						<div class="postBox">
							<div class="title">
								<!-- <div class="key">title</div>
								<div class="stick"></div> -->
								<input class="titlebox" v-model="form.title" type="string" placeholder="제목"/>
							</div>
							<div class="tagBox">
								<div class="tag">
									<select v-model="form.tag">
										<option disabled value="">소분/나눔</option>
										<option value=0>소분</option>
										<option value=1>나눔</option>
										<option value=2>완료</option>
									</select>
								</div>
								<div class="price">
									<!-- <div class="key">price</div>
									<div class="stick"></div> -->
									<label for="form-price">원</label>
									<input id="form-price" v-if="form.tag !== '1' && form.tag !== '2'" v-model="form.price" type="number" placeholder="-" min="0"/>
									<div class="zero" v-else> {{form.price = 0}} </div>
								</div>
							</div>
							<div class="textBox">
								<!-- <div>{{form.user}}</div> -->
								<!-- <div class="key">내용</div>
								<div class="stick"></div> -->
								<textarea class="text" v-model="form.content" type="string" placeholder="내용"/>
								<img class="thumbnail" :src="form.image1 ? url1 : post.image1" />
								<img class="thumbnail" :src="form.image2 ? url2 : post.image2" v-show="checkImg2"/>
								<img class="thumbnail" :src="form.image3 ? url3 : post.image3" v-show="checkImg3"/>
							</div>
							<div class="fileSelect">
								<label class="input-file-btn" for="input-file">사진 첨부하기</label>
								<input multiple @change="onInputImage()" ref="postImage" type="file" id="input-file" style="display: none"/>
								<p>이미지는 최대 3장까지</p>
							</div>
						</div>
						<div class="bntBox">
							<div>
								<button class="registerBtn" @click="update()">작성</button>
								<button class="cancelBtn" @click="cancel()">취소</button>
							</div>
						</div>
					</form>
				</div>
			</div>
		</div>
</template>

<script>
import { mapState, mapActions } from 'vuex';
import _ from 'lodash';

export default {
	name: 'UpdatePost',
	data() {
		return {
			form: {
				id: this.$route.params.id,
				title: '',
				tag: '',
				region: '',
				content: '',
				price: '',
				image1: undefined,
				image2: undefined,
				image3: undefined,
				view_count: ''
			},
			checkImg2: false,
			checkImg3: false,
			url1: '',
			url2: '',
			url3: ''
		}
	},
	computed: {
		...mapState('post', ['post'])
	},
	created : function() {
		this.form.title = this.post.title;
		this.form.tag = this.post.tag;
		this.form.price = this.post.price;
		this.form.content = this.post.content;
		this.form.region = this.post.region;
		this.form.view_count = this.post.view_count;
		this.checkImg2 = this.post.image2 ? true : false;
		this.checkImg3 = this.post.image3 ? true : false;
	},
	methods: {
		...mapActions('post', [
			'updatePost',
			'searchPostWithId',
		]),
		onInputImage() {
			this.form.image1 = this.$refs.postImage.files[0] ? this.$refs.postImage.files[0] : undefined;
			this.form.image2 = this.$refs.postImage.files[1] ? this.$refs.postImage.files[1] : '';
			this.form.image3 = this.$refs.postImage.files[2] ? this.$refs.postImage.files[2] : '';
			this.previewURL();
		},
		previewURL(e) {
			this.url1 = this.form.image1 ? URL.createObjectURL(this.form.image1) : '';
			this.url2 = this.form.image2 ? URL.createObjectURL(this.form.image2) : '';
			this.url3 = this.form.image3 ? URL.createObjectURL(this.form.image3) : '';
			this.checkImg2 = this.form.image2 ? true : false;
			this.checkImg3 = this.form.image3 ? true : false;
		},
		async update() {
			const postObj = {
				id: this.form.id,
				title: this.form.title,
				tag: this.form.tag,
				content: this.form.content,
				price: this.form.price,
				image1: this.form.image1,
				image2: this.form.image2,
				image3: this.form.image3
			};
			let formData = new FormData();

			for (let key in postObj) {
				!_.isNil(postObj[key]) &&	formData.append(key, postObj[key]);
			}
			await this.updatePost(formData);
      await this.$router.push({
        name: 'Post',
        params: {
          id: this.form.id,
        },
        query: {
          notFetching: '1',
        },
      });
		},
		cancel() {
			this.$router.push(`/post/${this.form.id}`);
		}
	}
}
</script>


<style lang="scss" scoped>
@import '../scss/main.scss';
@import '../scss/commons.scss';
@mixin center {
	justify-content: center;
	align-items: center;
	margin: auto;
	text-align: center;
}
@mixin boxCss {
	width: 100%;
	// max-width: 900px;
	margin: auto;
	border-bottom: 1px solid #ddd;
}
@mixin price {
	width: 90%;
	color: rgba(#76862c, 0.76);
	border-color: transparent;
	font-size: 15px;
}
@mixin input($size) {
	width: $size;
	padding: 8px;
	margin: 10px 0px;
	color: rgba(#76862c, 0.76);
	border-color: transparent;
}
@mixin btnCss {
	font-family: 'Gowun Dodum', sans-serif;
	font-size: 17px;
	width: 10%;
	min-width: 80px;
	height: 15%;
	padding: 8px;
	border-radius: .3em;
	border-color: rgba(187, 212, 68, 30%);
	box-shadow: 0 0 10px 0 $color_shadow_03;
	border: 20px;
}
.content {
	margin-bottom: 1em;
	.background{
		@include center;
		@media (min-width: 1000px) {
			width: 700px;
		}
		width: 70vw;
		@media (max-width: 500px) {
			width: 100%;
		}
		.createPostArea {
			@include center;
			.createInfo{
				margin: 10px 0px;
				font-size: 20px;
				color: rgba(#76862c, 0.76);
			}
			.postBox {
				width: 100%;
				height: 100%;
				margin: auto;
				display: block;
				.title {
					@include boxCss;
					.titlebox{
						@include input(95%);
						font-size: 1.1em;
					}
				}
				.tagBox {
					@include boxCss;
					display: flex;
					.tag{
						@include input(100%);
						select{
							width: 90%;
							text-align: center;
							color: rgba(#76862c, 0.76);
							border-color: transparent;
						}
					}
					.price{
						@include input(100%);
						position: relative;
						label {
							position: absolute;
							right: 3em;
							top: 28%;
							color: gray;
							font-size: .8em;
						}
						input{
							@include price;
							text-align: center;
							&:focus::placeholder {
  							color: transparent;
							}
						}
						.zero{
							@include price;
							margin: auto;
						}
					}
				}
				.textBox{
					@include boxCss;
					.text{
						@include input(95%);
						resize: none;
						height: 200px;
						font-size: .9em;
						// min-height: 300px;
					}
					.thumbnail{
						width: 80px;
						height: 80px;
					}
				}
				.fileSelect{
					width: 100%;
					padding: 8px;
					margin: 10px 0px;
					display: flex;
					.input-file-btn{
						font-family: 'Gowun Dodum', sans-serif;
						width: 50%;
						padding: 8px;
						margin: 0px 10px 0px 0px;
						color: rgba(#76862c, 0.76);
						border-radius: .3em;
						border: 1px solid #ddd;
						font-size: 15px;
					}
					p{
						width: 50%;
						margin: auto;
						color: rgba(#76862c, 0.76);
						border-color: transparent;
						text-align: center;
						font-size: 12px;
					}
				}
			}
			.bntBox {
				width: 100%;
				padding: 8px;
				display: flex;
				div{
					width: 100%;
					margin: auto;
					.registerBtn{
						@include btnCss;
						background-color: rgba(#76862c, 0.76);
						color: white;
					}
					.cancelBtn{
						@include btnCss;
						margin: 10px 0px 10px 10px;
						color: rgba(#76862c, 0.76);
					}
				}
			}
		}
	}
}
</style>
