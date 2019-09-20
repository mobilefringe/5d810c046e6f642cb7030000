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
        		
                <div class="site_container">
    				<div class="pages_content about_content">
    				    <div class="page_body description_text text_left" v-if="currentPage" v-html="currentPage.body"></div>
        
                    </div>
                </div><!--page content ends-->
                    
                    <!--<section style="background: #ededed; padding: 2em 15px; margin: 0px -15px;">-->
                    <!--    <div class="site_container">-->
                    <!--        <div class="subpage-1" v-if="section1">-->
                                
                    <!--            <h2 style="text-align: center;">{{section1.title}}</h2>-->
                    <!--            <div v-html="section1.body"></div>-->
                    <!--        </div>-->
                    <!--        <div class="subpage-2" v-if="section2">-->
                    <!--            <img src="//codecloud.cdn.speedyrails.net/sites/5d810c046e6f642cb7030000/image/png/1568923232000/recycling2.png" alt="">-->
                    <!--            <h2 style="text-align: center;">{{section2.title}}</h2>-->
                    <!--            <div v-html="section2.body"></div>-->
        
                    <!--        </div>-->
                    <!--    </div>-->
                    <!--</section>-->
                    <!--    <div class="subpage-3" v-if="section3">-->
                    <!--        <h2 style="text-align: center;">{{section3.title}}</h2>-->
                    <!--        <div v-html="section3.body"></div>-->
    
                    <!--    </div>-->
                    <!--    <div class="subpage-4" v-if="section4">-->
                    <!--        <h2 style="text-align: center;">{{section4.title}}</h2>-->
                    <!--        <div v-html="section4.body"></div>-->
    
                    <!--    </div>-->
                    <!--    <div class="subpage-5" v-if="section5">-->
                    <!--        <h2 style="text-align: center;">{{section5.title}}</h2>-->
                    <!--        <div v-html="section5.body"></div>-->
    
                    <!--    </div>-->
                    <!--    <div class="subpage-6" v-if="section6">-->
                    <!--        <h2 style="text-align: center;">{{section6.title}}</h2>-->
                    <!--        <div v-html="section6.body"></div>-->
    
                    <!--    </div>-->
                    <!--</section>-->


    		
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
                    // section1: {},
                    // section2: {},
                    // section3: {},
                    // section4: {},
                    // section5: {},
                    // section6: {},
                    pageBanner: {}
                }
            },
            created() {
                this.loadData().then(response => {
                    var temp_repo = this.findRepoByName('About Us Banner');
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
                    // if (response[0].data.subpages) {
                    //     this.section1 = this.currentPage.subpages[0];
                    //     this.section2 = this.currentPage.subpages[1];
                    //     this.section3 = this.currentPage.subpages[2];
                    //     this.section4 = this.currentPage.subpages[3];
                    //     this.section5 = this.currentPage.subpages[4];
                    //     this.section6 = this.currentPage.subpages[5];
                    // }
                    
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
