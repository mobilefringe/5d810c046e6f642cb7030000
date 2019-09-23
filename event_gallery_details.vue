<template>
    <div> <!-- without an outer container div this component template will not render -->
        <loader v-if="!dataLoaded"></loader>
        <transition name="fade">
            <div v-if="dataLoaded" v-cloak>
        		<div class="site_container">

        		    <div v-masonry transition-duration="0.3s" item-selector=".grid-item" > 
                        <div v-masonry-tile class="item">
                           <!-- <div v-for="feature in feature_items" :class="'grid-item ' + feature.masonry_class ">-->
                        	  <!--  <div v-if="feature.description" class="feature_item_container" >-->
                        			<!--<img :src="feature.image_url" alt="">-->
                        			<!--<div class="feature_item_info" >-->
                        			<!--	<div class="feature_item_content">-->
                        			<!--		<p v-if="feature.description">{{ feature.name }}</p>-->
                        			<!--		<h3>{{ feature.name }}</h3>-->
                        			<!--		<a class="feature_item_link" :href="feature.url">-->
                        			<!--		    <div class="feature_item_more animated_btn" :aria-label="feature.name">{{feature.description}}</div>-->
                        			<!--		</a>-->
                        			<!--	</div>-->
                        			<!--</div>-->
                        	  <!--  </div>-->
                        	  <!--  <div v-else-if="!feature.isSocial" class="feature_item_container">-->
                        	  <!--      <a :href="feature.url">-->
                           <!-- 			<img :src="feature.image_url" :alt="feature.name">-->
                            			
                           <!-- 			<div class="feature_item_info">-->
                            			
                           <!-- 			</div>-->
                            			
                           <!-- 		</a>-->
                        	  <!--  </div>-->
                           <!-- </div>-->
                        </div>
                    </div>
                    <!--<div v-masonry transition-duration="0.3s" item-selector=".grid-item" class="visible_phone">-->
                    <!--    <div v-masonry-tile class="item">-->
                    <!--        <div v-for="feature in feature_items" :class="'grid-item ' + feature.masonry_class ">-->
                    <!--    	    <div v-if="feature.name && feature.description" class="feature_item_container">-->
                    <!--    	        <a :href="feature.url">-->
                    <!--        			<img :src="feature.image_url" :alt="feature.name">-->
                    <!--        			<div class="feature_item_info" >-->
                    <!--        				<div class="feature_item_content">-->
                    <!--        					<p>{{ feature.name }}</p>-->
                    <!--        					<h3>{{ feature.description }}</h3>-->
                    <!--        					<div class="feature_item_more" :aria-label="feature.name">View Details</div>-->
                    <!--        				</div>-->
                    <!--        			</div>-->
                    <!--        		</a>-->
                    <!--    	    </div>-->
                    <!--    	    <div v-else class="feature_item_container">-->
                    <!--    	        <a :href="feature.url">-->
                    <!--        			<img :src="feature.image_url" :alt="feature.name">-->
                    <!--        			<div class="feature_item_info"></div>-->
                    <!--        		</a>-->
                    <!--    	    </div>-->
                    <!--        </div>-->
                    <!--    </div>-->
                    <!--</div>-->
                    
                    
                    <div class="home_page_title_container">
        		        <h5 class="home_page_subtitle center caps">Follow us on</h5>
        		        <h3 class="home_page_title second caps">instagram</h3>
        		    </div>
        		    
        		    <!--<div class="row">-->
        		    <div>
        		    
        		        <!--<div class="row hidden-xs margin_60">-->
        		        <div class="hidden-xs margin_60">
                            <div class="">
                                <div class="insta-feed-container">
                                    <div class="insta-feed-image " v-for="(item, index) in instaFeed">
                                        <a :href="item.link" target="_blank">
                                            <div class="insta-img" v-bind:style="{ 'background-image': 'url(' + item.images.standard_resolution.url + ')' }"></div>
                                            <div class="insta_content">
                                                <p class="insta_caption">{{ item.caption.text }}</p>
                                                <p class="insta_user">@{{ item.user.username }}</p>
                                                <i class="insta_icon fab fa-instagram"></i>
                                            </div>
                                        </a>
                                    </div>
                                </div>
                            </div>
                        </div>
                        <div class="visible-xs margin_60 insta-feed-container">
                            <div class="insta_prev"></div>
                            <slick ref="slick" :options="instaOptions">
                                <div class="insta-feed-image " v-for="(item, index) in instaFeed">
                                    <a :href="item.link" target="_blank">
                                        <div class="insta-img" v-bind:style="{ 'background-image': 'url(' + item.images.standard_resolution.url + ')' }"></div>
                                        <div class="insta_content">
                                            <p class="insta_caption">{{ item.caption.text }}</p>
                                            <p class="insta_user">@{{ item.user.username }}</p>
                                            <i class="insta_icon fab fa-instagram"></i>
                                        </div>
                                    </a>
                                </div>
                            </slick>
                            <div class="insta_next"></div>
                        </div>
                        
        		        <!--<div v-for="feature in programs" class="col-sm-6">-->
        		        <!--    <div class="feature_item_container programs" :class="feature.program_class">-->
                  <!--  			<div class="feature_item_info">-->
                  <!--  				<div class="feature_item_content">-->
                  <!--  					<p>{{ feature.name }}</p>-->
                  <!--  					<h3>{{ feature.description }}</h3>-->
                  <!--  					<a :href="feature.url" :aria-label="feature.name">-->
                  <!--      					<div class="feature_item_more animated_btn">View Details</div>-->
                  <!--      				</a>-->
                  <!--  				</div>-->
                  <!--  			</div>-->
                  <!--  	    </div>-->
        		        <!--</div>-->
        		    </div>
        		</div>
        		
        		
       
        	</div>
        </transition>    		
	</div>
</template>

<script>
    define(["Vue", "vuex", "vue!vue-slick", "js-cookie", "masonry", "vue-masonry-plugin", "vue!mapplic-map", "moment", "moment-timezone"], function(Vue, Vuex, slick, Cookies, masonry, VueMasonryPlugin, MapplicComponent, moment, tz) {
        Vue.use(VueMasonryPlugin.default);
        return Vue.component("home-component", {
            template: template, // the variable template will be injected
            
            props:['id', 'locale'],
            beforeRouteUpdate(to, from, next) {
                this.updateCurrentGallery(this.id);
                next();
            },
            data: function() {
                return {
                    dataLoaded: false,

                }
            },
            created () {
                this.loadData().then(response => {
                    var temp_repo = this.findRepoByName('Events Banner');
                    if (temp_repo) {
                        try {
                            this.pageBanner = temp_repo.images[0];
                        } catch(e) {
                            
                        }
                    } else {
                        this.pageBanner = { "image_url": "" }
                    }
                    
                    this.updatecurrentEvent(this.id);
                    this.dataLoaded = true;
                });
            },
            computed: {
                ...Vuex.mapGetters([
                    'property',
                    'findRepoByName',
                    'processedGalleries',
                    'findGalleryById',
                    'findGalleryBySlug'
                ])
            },
            methods: {
                loadData: async function() {
                    try {
                        let results = await Promise.all([
                            this.$store.dispatch("getData", "repos"),
                            this.$store.dispatch("getData", "fashions")
                        ]);
                        return results;
                    } catch (e) {
                        console.log("Error loading data: " + e.message);
                    }
                },
                
                updateCurrentGallery (id) {
                    this.currentGallery = this.findGalleryBySlug(id);
                    console.log(this.currentGallery)
                    if (this.currentGallery != null || this.currentGallery != undefined){
                        //customize json
                    } else {
                        this.$router.replace({ name: '404'});
                    }
                },

                
                
                
            }
        })
    })
</script>