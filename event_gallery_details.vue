<template>
    <div> <!-- without an outer container div this component template will not render -->
        <loader v-if="!dataLoaded"></loader>
        <transition name="fade">
            <div v-if="dataLoaded" v-cloak>
        		<div class="page_header" v-if="pageBanner" v-bind:style="{ backgroundImage: 'url(' + pageBanner.image_url + ')' }">
        			<div class="site_container">
        				<div class="header_content caps">
    						<!--<h1>{{ $t("jobs_page.jobs") }}</h1>-->
    						<h1>EVENT NAME HERE</h1>
        				</div>
        			</div>
        		</div>
        		<div class="site_container">
                    <p>DESCRIPTION HERE</p>

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
                this.updateCurrentJob(this.id);
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
                    // 'findGalleryById',
                    // 'findGalleryBySlug',
                    
                    'findEventBySlug'
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
                // mobile_feature_items () {
                //     var features = _.slice(this.$store.state.feature_items, 0, 6);
                //     _.forEach(features, function(value, key) {
                //         if ( _.includes([1], key) ) {
                //             value.masonry_class = "grid-item--width2";
                //             value.text_class = " ";
                //             value.border = " ";
                //         } else if ( _.includes([2], key) ){
                //             value.masonry_class = "grid-item--width2";
                //             value.text_class = "add_black";
                //             value.border = " ";
                //         } else if ( _.includes([3], key) ){
                //             value.masonry_class = "grid-item--height2";
                //             value.text_class = " ";
                //             value.border = " ";
                //         } else if ( _.includes([4], key) ){
                //             value.text_class = " ";
                //             value.border = "full_border";
                //         } else if ( _.includes([5], key) ){
                //             value.text_class = "add_black";
                //             value.border = " ";
                //             value.last_item = true;
                //         } else {
                //             value.masonry_class = " ";
                //             value.text_class = " ";
                //             value.border = " ";
                //             value.last_item = false;
                //         }
                //     });
                //     features = _.sortBy(features, [function(o) { return o.mobile_order; }]);
                //     return features;
                // }
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
                            this.$store.dispatch("getData", "repos"),
                            this.$store.dispatch("getData", "fashions")
                        ]);
                        return results;
                    } catch (e) {
                        console.log("Error loading data: " + e.message);
                    }
                }
                // updateCurrentJob (id) {
                //     this.currentJob = this.findJobBySlug(id);
                //     if (this.currentJob != null || this.currentJob != undefined){
                //         //customize json
                //     } else {
                //         this.$router.replace({ name: '404'});
                //     }
                // },
            }
        })
    })
</script>