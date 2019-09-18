<template>
    <div> <!-- without an outer container div this component template will not render -->
        <loader v-if="!dataLoaded"></loader>
        <transition name="fade">
            <div v-if="dataLoaded" v-cloak>
            
                <div class="page_header" v-if="pageBanner" v-bind:style="{ backgroundImage: 'url(' + pageBanner.image_url + ')' }">
        			<div class="site_container">
        				<div class="header_content caps">
        					<h1>{{ $t("events_page.events_header") }}</h1>
        				</div>
        			</div>
        		</div>	<!--page_header ends-->
        		
                <div class="site_container">
    				<div class="pages_content">
    				    <div>
    				        <img src="//codecloud.cdn.speedyrails.net/sites/5d810c046e6f642cb7030000/image/png/1568827247000/history.png" alt="Northgate Shopping Centre's historic timeline.">
    				    </div>
                        


                        <div>
                            <p>Northgate is anchored by Best Buy and Walmart and offers over 100 quality stores with a unique mix of both independent and national retailers such as Gap, American Eagle Outfitters, Boathouse, West49, People’s Jewelers, Body Shop and Eclipse.</p>
                            <p>Northgate Shopping Centre is currently managed by <a href="https://www.morguard.com/" target="_blank">Morguard</a>, a fully integrated real estate company. They own, manage and invest in high-quality, well-located, geographically diversified assets across North America, including a Retail portfolio of more than 18 million square feet of local, regional, super regional, mixed-use and development properties in more than 63 Canadian and U.S. communities. Morguard understands the real estate requirements of retailers with a focus to maximize traffic, sales, and market share. Their disciplined merchandising strategies ensure all aspects of their retail properties are optimized. To learn more, visit <a href="https://www.morguard.com/" target="_blank">morguard.com</a>.</p>
                        </div>
                        
                        <div>
                            <p>Northgate strives to provide a safe and pleasant shopping environment for all our shopping centre visitors, patrons and employees. Please take a moment to review our Code of Conduct.</p>
                        </div>
                    
                    
                        <div id="spare-change-div">
                            <h2>Turn Spare Change into a Northgate Gift Card</h2>
                            <p>It&#8217;s easy to turn your spare change into something you have been wishing for at Northgate!</p>
                            <h3 style="margin: 60px 0;">This is how you do it:</h3>
                            <ol id="change-machine" class="clearfix">
                                <li>Find the Northgate Coin Machine located near Customer Care</li>
                                <li style="background-position: left -2614px;">Press the green &#8220;Start&#8221; button</li>
                                <li style="background-position: left -2895px;">Deposit loose coins in the tray</li>
                                <li style="background-position: left -3178px;">Press &#8220;Print&#8221;</li>
                                <li style="background-position: left -3426px;">Remove the printed voucher</li>
                                <li style="background-position: left -3691px;">Redeem your voucher at Customer Care</li>
                            </ol>
                        </div> <!--spare change div ends-->
                        
                        <section class="clearfix" style="background: #f6f6f6; padding: 3em 15px; margin: 0px -15px;">
                            <div id="important-note-div">
                                <h2 style="color: #666;">It&#8217;s important to note the following:</h2>
                                <ul class="clearfix" style="font-family: Helvetica,Arial,sans-serif;">
                                    <li>You can deposit mixed coins</li>
                                    <li>A minimum of $6.00 is required per transaction</li>
                                    <li>Remember to check the coin reject tray for discarded coins</li>
                                    <li>A $1.00 administration fee is applicable per transaction and is deducted from the total change deposited</li>
                                </ul>
                                <p style="color: #666;">The Northgate Change Machine and Guest Services are located inside the parking garage entrance.</p>
                            </div>
                        </section>  <!--important to note the following ends-->
	

                    </div>  <!--page content ends-->

            
        
    				
    				
    				
    				    
    				
    			</div> 	<!--site container ends-->
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
                    dataLoaded: true,
                    currentPage: null
                }
            },
            created() {
                // this.updateCurrentPage(this.id);
            },
            // watch: {
            //     $route: function () {
            //         this.updateCurrentPage(this.$route.params.id);
            //     }
            // },
            computed: {
                ...Vuex.mapGetters([
                    'property'
                ])
            }
            // methods: {
            //     updateCurrentPage(id) {
            //         var _this = this;
            //         this.$store.dispatch('LOAD_PAGE_DATA', { url: this.property.mm_host + "/pages/" + this.id + ".json" }).then(function (response) {
            //             _this.currentPage = response.data;
            //             _this.dataLoaded = true;
            //         }, function (error) {
            //             console.error( "Could not retrieve data from server. Please check internet connection and try again.");
            //             _this.$router.replace({ name: 'home' });
            //         });
            //     }
            // }
        });
    });
</script>
