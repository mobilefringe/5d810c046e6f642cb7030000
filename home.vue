<template>
    <div> <!-- without an outer container div this component template will not render -->
        <loader v-if="!dataLoaded"></loader>
        <transition name="fade">
            <div v-if="dataLoaded" v-cloak>
        		<div class="banner_div">
        			<div class="home_banner_container">
        			
        			    <!--if multiple home hero banners-->
        				<slick v-if="banners.length > 1" ref="slick" :options="slickOptions">
        					<div class="" v-for="banner in banners" v-if="banners">
        					    <a v-if="banner.url" :href="banner.url">
        					        <div class="home_banner" v-bind:style="{ backgroundImage: 'url(' + banner.image_url + ')' }">
            						    <div class="home_banner_content" v-if="banner.name && banner.description">
            						        <h1>{{ banner.name }}</h1>
            						        <h2>{{ banner.description }}</h2>
            						    </div>   
            						</div>
        					    </a>
        					    
        						<div v-else class="home_banner" v-bind:style="{ backgroundImage: 'url(' + banner.image_url + ')' }">
        						    <div class="home_banner_content" v-if="banner.name && banner.description">
        						        <h1>{{ banner.name }}</h1>
        						        <h2>{{ banner.description }}</h2>
        						    </div>   
        						</div>
        					</div>
        				</slick>
        				
        				<!--if only 1 banner-->
    					<div class="" v-for="banner in banners" v-if="banners.length == 1">
    					    <a v-if="banner.url" :href="banner.url">
    					        <div class="home_banner" v-bind:style="{ backgroundImage: 'url(' + banner.image_url + ')' }">
        						    <div class="home_banner_content" v-if="banner.name && banner.description">
        						        <h1>{{ banner.name }}</h1>
        						        <h2>{{ banner.description }}</h2>
        						    </div>   
        						</div>
    					    </a>
    					
    						<div v-else class="home_banner" v-bind:style="{ backgroundImage: 'url(' + banner.image_url + ')' }">
    						    <div class="home_banner_content" v-if="banner.name && banner.description">
    						        <h1>{{ banner.name }}</h1>
    						        <h2>{{ banner.description }}</h2>
    						    </div>   
    						</div>
    					</div>
        				
        			</div>
        		</div>
        		<div class="site_container">
        		    
        		    <!--mobile view: static image of map that is clickable to go to /maps-->
        		    
        		    <div class="home_page_title_container">
        		        <h4 class="home_page_subtitle center caps">Explore</h4>
        		        <h3 class="home_page_title caps">{{ property.name }}</h3>
        		    </div>
        		    <div v-masonry transition-duration="0.3s" item-selector=".grid-item" > <!-- class="hidden_phone" -->
                        <div v-masonry-tile class="item">
                            <div v-for="feature in feature_items" :class="'grid-item ' + feature.masonry_class ">
                        	    <div v-if="feature.description" class="feature_item_container" >
                        			<img :src="feature.image_url" alt="">
                        			<div class="feature_item_info" >
                        				<div class="feature_item_content">
                        					<h3>{{ feature.name }}</h3>
                        					<a class="feature_item_link" :href="feature.url">
                        					    <div class="feature_item_more animated_btn" :aria-label="feature.name">{{feature.description}}</div>
                        					</a>
                        				</div>
                        			</div>
                        	    </div>
                        	    <div v-else-if="!feature.isSocial" class="feature_item_container">
                        	        <a :href="feature.url">
                            			<img :src="feature.image_url" :alt="feature.name">
                            			
                            			<div class="feature_item_info">
                            			
                            			</div>
                            			
                            		</a>
                        	    </div>
                        	    <div v-else class="feature_item_container feat_social">
                        	        
                        			<img :src="feature.image_url" :alt="feature.name">
                        			<div class="feature_item_info">
                        			    <h3>Follow Us</h3>
                        			    <div class="social_icons">
                                            <span v-for="item in social_media">
                                                <a :href="item.url" target="_blank">
                                                    <p class="accessibility">{{ item.name }}</p>
                                                    <i :class="item.iconClass" aria-hidden="true"></i>
                                                </a>
                                            </span>
                                        </div>
                        			</div>
                        	    </div>
                            </div>
                        </div>
                    </div>

                    
                    
                    <div class="home_page_title_container">
        		        <h4 class="home_page_subtitle center caps">Follow us on</h4>
        		        <h3 class="home_page_title second caps">instagram</h3>
        		    </div>
        		    

        		    <div>
        		        <div class="hidden-xs margin_85">
                            <div class="">
                                <div class="insta-feed-container">
                                    <div class="insta-feed-image " v-for="(item, index) in instaFeed">
                                        <a :href="item.link" target="_blank">
                                            <div class="insta-img" v-bind:style="{ 'backgroundImage': 'url(' + item.images.standard_resolution.url + ')' }"></div>
                                            <div class="insta_content">
                                                <p class="insta_caption">{{ item.caption.text }}</p>
                                                <p class="insta_user">@{{ item.user.username }}</p>
                                                <i class="insta_icon fa fa-instagram"></i>
                                            </div>
                                        </a>
                                    </div>
                                </div>
                            </div>
                        </div>
                        <div class="visible-xs insta-feed-container">
                            <div class="insta_prev"></div>
                            <slick ref="slick" :options="instaOptions">
                                <div class="insta-feed-image " v-for="(item, index) in instaFeed">
                                    <a :href="item.link" target="_blank">
                                        <div class="insta-img" v-bind:style="{ 'backgroundImage': 'url(' + item.images.standard_resolution.url + ')' }"></div>
                                        <div class="insta_content">
                                            <p class="insta_caption">{{ item.caption.text }}</p>
                                            <p class="insta_user">@{{ item.user.username }}</p>
                                            <i class="insta_icon fa fa-instagram instagram"></i>
                                        </div>
                                    </a>
                                </div>
                            </slick>
                            <div class="insta_next"></div>
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
            props:['locale', "social_media"],
            data: function() {
                return {
                    dataLoaded: false,
                    show_popup: false,
                    popup: null,
                    slickOptions: {
                        arrows: false,
                        autoplay: true,
                        autoplaySpeed: 3000,
                        cssEase: 'linear',
                        dots: true,
                        fade: true,
                        infinite: true,
                        slidesToShow: 1,
                        speed: 1200
                    },
                    isMobile: false,
                    suggestionAttribute: "name",
                    storeSearch: null,
                    
                    instaOptions: {
                        arrows: true,
                        autoplay: true,
                        autoplaySpeed: 8000,
                        cssEase: 'linear',
                        dots: false,
                        fade: true,
                        infinite: true,
                        slidesToShow: 1,
                        speed: 1000,
                        nextArrow: '.insta_next',
                        prevArrow: '.insta_prev'
                    },
                    instaFeed: null
                }
            },
            created () {
                this.loadData().then(response => {

                    var socialFeed = response[3].data;
                    var social_feed = socialFeed.social.instagram;
                    this.instaFeed = _.slice(social_feed, [0], [6]);
                    this.instaFeed.map(insta => {
                        if(insta.caption != null){
                            insta.caption.text = _.truncate(insta.caption.text, { 'length': 99, 'separator': ' ' });
                        }
                    });

                    this.dataLoaded = true;  
                });
                window.addEventListener("resize", this.getWindowWidth);
                  //Init
                  this.getWindowWidth();
            },
            watch : {
                dataLoaded () {
                    var viewed = Cookies.get('popup_viewed');
                    if(this.popup !== null && viewed !== "true") {
                        Cookies.set('popup_viewed', "true");
                        viewed = Cookies.get('popup_viewed');
                        this.show_popup = true;
                        this.popup.image_url = "//mallmaverick.cdn.speedyrails.net" + this.popup.photo_url;
                        document.getElementById('popup_backdrop').style.display = "block";
                    } else {
                        document.getElementById('popup_backdrop').style.display = "none";
                    }
                }
            },
            computed: {
                ...Vuex.mapGetters([
                    'property',
                    'timezone',
                    'getPropertyHours',
                    'getPropertyHolidayHours',
                    'processedStores'
                ]),
                banners () {
                    var banners = [];//this.$store.state.banners;
                    _.forEach(this.$store.state.banners, function(value, key) {
                        var today = new Date();
                        var start = new Date (value.start_date);
                        if (start <= today){
                            if (value.end_date){
                                var end = new Date (value.end_date);
                                if (end >= today){
                                    banners.push(value);  
                                }
                            } else {
                                banners.push(value);
                            }
                            console.log(banners)
                        }
                    })
                    return _.orderBy(banners, ['position'], ['asc']);
                },
                feature_items () {
                    var features = _.slice(this.$store.state.feature_items, 0, 6);
                    _.forEach(features, function(value, key) {
                        if ( _.includes([1], key) ) {
                            value.masonry_class = "grid-item--width2";
                        } else if ( _.includes([2], key) ){
                            value.masonry_class = "grid-item--width2";
                        } else if ( _.includes([3], key) ){
                            value.masonry_class = "grid-item--height2";
                        } else {
                            value.masonry_class = " ";
                        }
                        if (_.includes(value.name , "Social Media")) {
                            value.isSocial = true
                        } else {
                            value.isSocial = false
                        }
                    });
                    return features;
                },
                programs () {
                    var features = _.slice(this.$store.state.feature_items, 6, 8); 
                    _.forEach(features, function(value, key) {
                        if ( _.includes([1], key)) {
                            value.program_class = "last";
                        }
                    });
                    return features
                },
                allStores() {
                    var all_stores = this.processedStores;
                    _.forEach(all_stores, function(value, key) {
                        value.zoom = 2;
                    });
                    var initZoom = {};
                    initZoom.svgmap_region = "init";
                    initZoom.z_coordinate = 1;
                    initZoom.x = 0.5;
                    initZoom.y = 0.5;
                    initZoom.zoom = 1;
                    all_stores.push(initZoom)
                    return all_stores
                },
                getSVGMap() {
                    var mapURL = "https://www.mallmaverick.com" + this.property.svgmap_url.split("?")[0];
                    return mapURL
                },
                floorList() {
                    var floor_list = [];
                    var floor_1 = {};
                    floor_1.id = "first-floor";
                    floor_1.title = "Level One";
                    floor_1.map = this.getSVGMap;
                    floor_1.z_index = 1;
                    floor_1.show = true;
                    floor_list.push(floor_1);
                    return floor_list;
                },
                todaysHours() {
                    var timezone = this.timezone
                    var regHours = this.getPropertyHours;
                    var holHours = this.getPropertyHolidayHours;
                    var hours = [];
                    // First check if there is a holiday today
                    _.forEach(holHours, function(value) {
                        var today = moment().format("YYYY-MM-DD");
                        var holiday_date = moment(value.holiday_date).tz(timezone).format("YYYY-MM-DD");
                        if (today == holiday_date) {
                            if (value.is_closed) {
                                value.is_open = false;
                            } else {
                                value.is_open = true;
                            }
                            hours.push(value);
                        }
                    });
                    // If there is no holiday today, check for today's hours
                    if (hours.length > 0) {
                        return hours;
                    } else {
                        _.forEach(regHours, function(value) {  
                            var today = moment().day();
                            if ( today == value.day_of_week ) {
                                var time = moment().tz(timezone).format('HH:mm');
                                var opens = moment(value.open_time).tz(timezone).format('HH:mm');
                                var closes = moment(value.close_time).tz(timezone).format('HH:mm');
                                if (time > opens && time < closes) {
                                    value.is_open = true;
                                } else {
                                    value.is_open = false;
                                }
                                hours.push(value);
                            }         
                        });
                        return hours;
                    }
                },
            },
            methods: {
                loadData: async function() {
                    try {
                        let results = await Promise.all([
                            this.$store.dispatch("getData", "banners"), 
                            this.$store.dispatch("getData", "feature_items"), 
                            this.$store.dispatch("getData", "popups"),
                            this.$store.dispatch('LOAD_PAGE_DATA', { url: "https://northgate.mallmaverick.com/api/v4/northgate/social.json" })
                        ]);
                        return results;
                    } catch (e) {
                        console.log("Error loading data: " + e.message);
                    }
                },
                onOptionSelect(option) {
                    this.$nextTick(function() {
                        this.storeSearch = ""
                    });
                    this.$refs.mapplic_ref.showLocation(option.svgmap_region);
                },
                getWindowWidth(event) {
                  var windowWidth = window.innerWidth;
                  if (windowWidth <= 768) {
                    this.isMobile = true;
                  } else {
                    this.isMobile = false;
                  }
                },
            },
            
            beforeDestroy: function() {
                window.removeEventListener("resize",  this.getWindowWidth);
            }
        })
    })
</script>