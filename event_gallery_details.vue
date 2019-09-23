<template>
    <div> <!-- without an outer container div this component template will not render -->
        <loader v-if="!dataLoaded"></loader>
        <transition name="fade">
            <div v-if="dataLoaded" v-cloak>
                <div class="page_header" v-if="pageBanner" v-bind:style="{ backgroundImage: 'url(' + pageBanner.image_url + ')' }">
        			<div class="site_container">
        				<div class="header_content caps">
    						<h1>HEADER HERE</h1>
        				</div>
        			</div>
        		</div>
        		<div class="margin_60"></div>
                <div v-if="dataLoaded" v-cloak class="site_container">
                    
            		
    		    </div>
    		</div>
		</transition>
	</div>
</template>

<script>
//     define(['Vue', 'vuex', 'moment', 'vue-lazy-load'], function(Vue, Vuex, moment, VueLazyload) {
//         Vue.use(VueLazyload);
//         return Vue.component("job-details-component", {
//             template: template, // the variable template will be injected,
//             data: function() {
//                 return {
//                     dataLoaded: false,
//                     pageBanner:null,
//                     currentJob: null,
//                     storeJobs : null
//                 }
//             },
//             props:['id', 'locale'],
//             beforeRouteUpdate(to, from, next) {
//                 this.updateCurrentJob(this.id);
//                 next();
//             },
//             created(){
//                 this.loadData().then(response => {
//                     var temp_repo = this.findRepoByName('Jobs Banner');
//                     if(temp_repo) {
//                         try {
//                             this.pageBanner = temp_repo.images[0];
//                         } catch(e) {
                            
//                         }
//                     } else {
//                         this.pageBanner = { "image_url": "" }
//                     }
//                     this.updateCurrentJob(this.id);
//                     this.dataLoaded = true;
//                 });
//             },
//             computed: {
//                 ...Vuex.mapGetters([
//                     'property',
//                     'timezone',
//                     'findRepoByName',
//                     'findJobBySlug',
//                     'findCategoryById'
//                 ])
//             },
//             methods: {
//                 updateCurrentJob (id) {
//                     this.currentJob = this.findJobBySlug(id);
//                     if (this.currentJob != null || this.currentJob != undefined){
//                         var property_name = this.property.name
//                         if (this.currentJob.store != null && this.currentJob.store != undefined){
//                             this.currentJob.store_name = this.currentJob.store.name;
//                             this.currentJob.store_phone = this.currentJob.store.phone
//                             this.currentJob.store_website = this.currentJob.store.website
                        
//                             // STORE LOGO
//                             if (_.includes(this.currentJob.store.store_front_url_abs, 'missing')) {
//                                 this.currentJob.no_logo = true
//                             }

//                             else {
//                                 this.currentJob.no_logo = false
//                                 this.currentJob.store_url = this.currentJob.store.image_url;
//                             }

//                         } else if (this.currentJob.store == null || this.currentJob.store == undefined) {
//                             this.currentJob.store = {};
//                             this.currentJob.no_logo = false;
//                             this.currentJob.store_url =  this.property.default_logo_url;
//                             this.currentJob.store_name = property_name;
//                         }
//                     } else {
//                         this.$router.replace({ name: '404'});
//                     }
//                 },
//                 loadData: async function() {
//                     try {
//                         let results = await Promise.all([this.$store.dispatch("getData", "repos"), this.$store.dispatch("getData", "jobs"), this.$store.dispatch("getData", "categories")]);
//                     } catch (e) {
//                         console.log("Error loading data: " + e.message);
//                     }
//                 },
//                 checkJobType(promo) {
//                     if(this.locale != "en-ca") {
//                         if(promo.job_type == "Full Time"){
//                             return "Plein Temps"
//                         } else if (promo.job_type == "Part Time"){
//                             return "Mi Temps"
//                         } else if (promo.job_type == "Full Time/Part Time"){
//                             return "Plein/Mi Temps"
//                         } else if (promo.job_type == "Seasonal"){
//                             return "Travail Saisonnier"
//                         }
//                     } else {
//                         return promo.job_type
//                     }
//                 },
//                 storeCategory(currentJob) {
//                     if (currentJob.store.length > 0) {
//                         var currentStoreCategory = currentJob.store.categories[0];
//                         category = this.findCategoryById(currentStoreCategory);
//                         return category.name
//                     }
//                 }
//             }
//         });
//     });
// </script>

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