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