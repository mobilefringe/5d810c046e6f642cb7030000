<template>
    <div> <!-- without an outer container div this component template will not render -->
        <loader v-if="!dataLoaded"></loader>
        <transition name="fade">
            <div v-if="dataLoaded" v-cloak>
                <div class="page_header" v-if="pageBanner" v-bind:style="{ backgroundImage: 'url(' + pageBanner.image_url + ')' }">
        			<div class="site_container">
        				<div class="header_content caps">
    						<h1>TEST: Gallery name</h1>
        				</div>
        			</div>
        		</div>
        		<div class="site_container">
        		    <!--<div v-masonry transition-duration="0.3s" item-selector=".grid-item" > -->
              <!--          <div v-masonry-tile >-->

              <!--              <div class="grid-item" v-if="galleries.length > 0" v-for="item in galleries" :key="item.id">-->
              <!--                  <img :src="item.firstImageUrl" :alt="item.name" />-->
                                        
              <!--                  <h3>{{item.name}}</h3>-->
                

                                
                                <!--set a link to the details per gallery-->
              <!--                  <router-link :to="'/event_photos/'+ item.slug" class="more-promo-link">-->
    							   <!--<div class="promo_learn_more animated_btn">View Gallery Here <span class="fa fa-caret-right"></span></div>-->
    						    <!--</router-link>-->
                                
              <!--                  <social-sharing :url="$root.shareURL('events',item.slug)" :title="item.title" :description="item.body" :quote="_.truncate(item.description, {'length': 99})" :twitter-user="$root.twitter_user" :media="item.image_url" inline-template >-->
            		<!--				<div class="blog-social-share pull-right">-->
            		<!--					<div class="social_share">-->
            		<!--						<network network="facebook">-->
            		<!--							<i class="fa fa-facebook social_icons" aria-hidden="true"></i>-->
            		<!--						</network>-->
            		<!--						<network network="twitter">-->
            		<!--							<i class="fa fa-twitter social_icons" aria-hidden="true"></i>-->
            		<!--						</network>-->
            		<!--					</div>-->
            		<!--				</div>-->
            		<!--			</social-sharing>-->
                    					
                    		<!--</div>	-->
              <!--          </div>-->
              <!--      </div>-->
              
                    <div v-masonry transition-duration="0.3s" item-selector=".grid-item" > <!-- class="hidden_phone" -->
                        <div v-masonry-tile class="item">
                           <!-- <div v-for="gallery-pic in galleries" :class="'grid-item ' + feature.masonry_class ">-->
                        	  <!--  <div class="feature_item_container" >-->
                        			<!--<img :src="gallery-pics" alt="">-->
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
                        	  <!--  <div v-else class="feature_item_container">-->
                        	        
                        			<!--<img :src="feature.image_url" :alt="feature.name">-->
                        			<!--<div class="feature_item_info">-->
                        			<!--    <h3>Follow Us</h3>-->
                        			<!--    <div class="social_icons">-->
                           <!--                 <span v-for="item in social_media">-->
                           <!--                     <a :href="item.url" target="_blank">-->
                           <!--                         <p class="accessibility">{{ item.name }}</p>-->
                           <!--                         <i :class="item.iconClass" aria-hidden="true"></i>-->
                           <!--                     </a>-->
                           <!--                 </span>-->
                           <!--             </div>-->
                        			<!--</div>-->
                        	  <!--  </div>-->
                           <!-- </div>-->
                        </div>
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
                    pageBanner: null,
                    currentGallery: null

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
                    
                    this.updateCurrentGallery(this.id);
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
                ]),
                galleries() {
                    var allGalleries = this.processedGalleries;
                    // var event_name = null;
                    // var firstImageUrl = null;
                    // var _this = this;
                    // _.forEach(allGalleries, function(value, key) {
                    //     if (value.fashion_images.length > 0) {
                    //         value.firstImageUrl = _this.property.mm_host + value.fashion_images[0].photo_url
                    //         if(value.name != null && value.name != undefined) {
                    //             // value.event_name = value.event.name;
                    //         }
                    //      }
                    // });
                    return allGalleries
                }
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
                
                updateCurrentGallery(id) {
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