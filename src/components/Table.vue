
<template>
	<div class="wrap">
		<div class="inner-wrap">
			<div class="tabs">
	            <router-link 
	                class="tab incompeleted"
	                :class="{'router-link-active': $route.fullPath ==='/main/'}" 
	                :to="{ name: 'dataTable' , params:{type:'incompeleted'} }"
	                @click.native="getData('incompeleted')">
	                Incompeleted notes
	                <span class="badge">{{ badgeCount[0] }}</span>
	            </router-link>
	            <router-link 
	                class="tab compeleted" 
	                :to="{ name: 'dataTable' , params:{type:'compeleted'} }"
	                @click.native="getData('compeleted')">
	                Compeleted notes
	                <span class="badge">{{ badgeCount[1] }}</span>
	            </router-link>
	            <router-link 
	                class="tab pending"
	                :to="{ name: 'dataTable' , params:{type:'pending'} }"
	                @click.native="getData('pending')">
	                Pending approval
	                <span class="badge">{{ badgeCount[2] }}</span>
	            </router-link>
	            <router-link 
	                class="tab followed" 
	                :to="{ name: 'dataTable' , params:{type:'followed'} }"
	                @click.native="getData('followed')">
	                Followed doctors
	                <span class="badge">{{ badgeCount[3] }}</span>
	            </router-link>
	        </div>
			<div class="table-container">
				<table class="display responsive-table">
				    <thead>
				        <tr>
				        	<th v-for="tableTitle in lists.tableTitle">
				        		{{ tableTitle }}
				        	</th>
				        	<th>Action</th>
				        </tr>
				    </thead>

				    <tbody>
				        <tr v-for="(list, index) in lists.tableContent.slice(pageStart, pageStart + countOfPage)">
				        	<td>{{ (currPage-1) * countOfPage + index + 1 }}</td>
				            <td>{{ list.id }}</td>
				            <td>{{ list.name }}</td>
				            <td v-if="list.date">{{ list.date }}</td>
				            <td v-if="list.department">{{ list.department }}</td>
				            <td class="actions">
				            	<i v-if="!list.department" class="fa fa-eye"></i>
				                <i v-if="!list.department" class="fa fa-pen"></i>
				                <i class="fa fa-star" v-if="list.department" @click.prevent="unfollow(list)"></i>
				            </td>
				        </tr>
				    </tbody>
				</table>
				<timePicker v-if="this.$route.params.type == 'compeleted'"></timePicker>
			</div>
		</div>
		<ul class="paginate">
	        <li class="prev" 
	            v-bind:class="{'disabled': (currPage === 1)}" 
	            @click.prevent="setPage(currPage-1)"
	            @><a href="#">Prev</a></li>
	        <li v-for="n in totalPage"
	            v-bind:class="{'active': (currPage === (n))}" 
	            @click.prevent="setPage(n)"><a href="#">{{n}}</a></li>
	        <li class="next"
	            v-bind:class="{'disabled': (currPage === totalPage)}" 
	            @click.prevent="setPage(currPage+1)"><a href="#">Next</a></li>
	      </ul>
	</div>
</template>

<script>

	import Vue from 'vue'
	Vue.component('timePicker', require('@/components/TimePicker.vue').default)

	export default{
		props:['lists', 'badgeCount'],
		data(){
            return {
                countOfPage: 8,
                currPage: 1,
            }
        },
        computed: {
            pageStart: function(){
                return (this.currPage - 1) * this.countOfPage;
              },
            totalPage: function(){
                return Math.ceil(this.lists.tableContent.length / this.countOfPage);
            }
        }, 
		methods:{
			unfollow(list){
				this.$emit("unfollow", list);
			},
			getData(v){
				this.$emit('selectedType', v);
				this.pageStartsFrom = 0;
				this.currPage = 1;
			},
			setPage: function(idx){
              if( idx <= 0 || idx > this.totalPage ){
                return;
              }
              this.currPage = idx;
            },
		}
	}
</script>

<style lang="scss" scoped>

	@import '../assets/global.scss';
	@import '../assets/paginate.scss';

	.wrap{
	  	width: 100%;
	  	.inner-wrap {
	  		display: flex;
	  		padding: 25px;
	  		@media screen and (max-width: 1024px) {
                padding: 0;
            }
	  		@media screen and (max-width: $break-small) {
                flex-direction: column;
            }
	  		.tabs {
            	margin: 0 40px 0 0;
	            @media screen and (max-width: $break-small) {
	                margin: 0 0 20px 0;
	            }
	            .tab {
	                cursor: pointer;
	                padding: 10px 10px 10px 0;
	                display: flex;
	                align-items: center;
	                justify-content: space-between;
	                min-width: 200px;
	                text-transform: uppercase;
	                border-right: 1px solid #ccc;
	                margin-right: -1px;
	                @media screen and (max-width: $break-small) {
	                    padding: 10px 0;
	                    position: relative;
	                    justify-content: space-between;
	                    border-right: none;
	                }
					&:hover {
						text-decoration: none;
	                	color: $black;
					}
	                .badge{
	                	padding: 3px;
	                    font-size: 12px;
	                    font-weight: 300;
	                    background: $brown;
	                    color: #fff;
	                }
	                &.router-link-active{
	                    border-right: 3px solid $brown;
	                    margin-right: -3px;
	                    color: $brown;
	                    font-weight: 600;
	                    @media screen and (max-width: $break-small) {
	                       border-right: none;
	                       margin-right: 0;
	                       border-bottom: 3px solid $brown;
	                    }
	                }
	            }  
	        } 
	        .table-container {
	        	width: 100%;
	        	min-height: 250px;
	        	table {
					width: 100%;

					th{ 
						background: $brown; 
						color: #fff;
					}
					th, td{
					  	text-align: left;
					  	border-bottom: 1px solid #ddd;
					  	padding: .4em 1em;
					}
					.actions {
						i {
							color: $blue;
							cursor: pointer;
							width: 30px;
							height: 30px;
							margin-right: 10px;
							line-height: 30px;
						}	
					}
					@media screen and (max-width: $break-medium){
						thead{
					     	display: none; 
					    }
					    th, 
					    td{
						    padding: .5em;
						    display: block;
						}
						tr{
							display: block;
							position: relative;
			        		margin-bottom: 1.5em;
						}
						td:nth-child(1), td:nth-child(4), td:nth-child(5) {
							display: none;
						}
						td:nth-child(2):before,
						td:nth-child(3):before{
						    position: absolute;
						    left: .5em;
						  }
						td:nth-child(2):before{
							content: 'Id';
						}
						td:nth-child(3):before{
			    			content: 'Name';
			  			}
			  			td:nth-child(2), 
			  			td:nth-child(3){
							padding-left: 35%;
						}
						td:nth-child(2){
							background: $brown;
			        		color: #fff;
						}
						td:nth-child(3){
							border-left: 1px solid #ddd;
							border-right: 1px solid #ddd;
						}

			  			td.actions{
					        position: absolute;
					        top: 0;
					        right: 0;
					        border: none;
					        background: none;
					        i {
					        	color: #fff;
					        }
					    }
					}
				}
	        }
		}
	}
</style>