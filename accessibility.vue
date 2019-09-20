<template>
    <div> <!-- without an outer container div this component template will not render -->
        <loader v-if="!dataLoaded"></loader>
        <transition name="fade">
            <div v-if="dataLoaded" v-cloak>
            
                <div class="page_header" v-if="pageBanner" v-bind:style="{ backgroundImage: 'url(' + pageBanner.image_url + ')' }">
        			<div class="site_container">
        				<div class="header_content caps">
        					<!--<h1>{{ $t("events_page.events_header") }}</h1>-->
        					<h1>Accessibility</h1>
        				</div>
        			</div>
        		</div>	<!--page_header ends-->
        		
        		<section class="accessibility_container site_container">
        		    <div class="wheelchair_image">
        		        <img :src="currentPage.image_url" alt="">
        		    </div>
        		    
        		    <div class="site_container">
        				<div class="pages_content about_content">
        				    <div class="page_body description_text text_left" v-if="currentPage" v-html="currentPage.body"></div>
            
                        </div>
                    </div><!--page content ends-->
        		</section>
        		


    		
			</div>	<!--data loaded ends-->
		</transition>
    </div>
</template>



<script>
    define(["Vue", "vuex"], function(Vue, Vuex) {
        return Vue.component("guest-services-component", {
            template: template, // the variable template will be injected,
            props:['id', 'locale'],
            data: function() {
                return {
                    dataLoaded: false,
                    currentPage: null,
                    pageBanner: {}
                }
            },
            created() {
                this.loadData().then(response => {
                    var temp_repo = this.findRepoByName('Accessibility Banner');
                    if (temp_repo) {
                        try {
                            this.pageBanner = temp_repo.images[0];
                        } catch(e) {
                            
                        }
                    } else {
                        this.pageBanner = { "image_url": "" }
                    }

                    this.currentPage = response[0].data;
                    console.log(this.currentPage)
                    
                    this.dataLoaded = true;
                });
            },

            computed: {
                ...Vuex.mapGetters([
                    'property',
                    'findRepoByName'
                ])
            },
            
            methods: {
                loadData: async function(id) {
                    try {
                        let results = await Promise.all([
                            this.$store.dispatch('LOAD_PAGE_DATA', { url: this.property.mm_host + "/pages/northgate-accessibility.json" }),
                            this.$store.dispatch("getData", "repos")]);
                        return results;
                    } catch (e) {
                        console.log("Error loading data: " + e.message);
                    }
                }
            }

        });
    });
</script>
