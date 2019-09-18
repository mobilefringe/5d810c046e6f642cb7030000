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
        		</div>
        		
                <div class="site_container">
    				<div class="pages_content">
    				    <h2 style="background: #f9f9f9; color: #666; border-bottom: 1px solid #eee; width: 100%; font-size: 16px !important; text-align: center; padding: 15px 15px; text-transform: uppercase; margin: 0 -15px;">Located near Center Court</h2>
    				    
    				    
                        <h3 style="text-align: center; max-width: 600px; margin: 0 auto; font-weight: normal; letter-spacing: 1px; line-height: 1.5em; margin-top: 1em;">Now located by the <strong>Parking Garage entrance</strong></h3>
<ul id="services_list" class="clearfix">
<li style="background-position: center -1219px;">Stroller, wheelchair &amp; scooter rentals</li>
<li style="background-position: center -789px;">Portable cell phone chargers</li>
<li style="background-position: center -653px;">Shopping Centre Information</li>
<li style="background-position: center -960px;">Northgate<br />
Gift Cards</li>
<li style="background-position: center -169px;">Security Services</li>
<li style="background-position: center -1088px;">Lost &amp; Found</li>
</ul>
<div id="wheelchair-services">
<h2>Accessibility</h2>
<p>Notices of temporary disruption will be posted to the Northgate Shopping Centre website in compliance with Morguard’s obligations under the Accessibility for Ontarians with Disabilities Act. To view a copy of Morguard’s Accessibility Policy please visit our website, at <a href="http://www.morguard.com/accessibility" target="_blank">http://www.morguard.com/accessibility</a><br />
To provide feedback regarding Morguard’s Accessibility Policy, please contact us:<br />
<strong>E-mail:</strong> <a href="mailto:accessibility@morguard.com">accessibility@morguard.com</a><br />
<strong>Local:</strong> <a href="tel:+17054728110">705-472-8110</a><br />
<strong>Head Office:</strong> <a href="tel:+19052813800">905-281-3800</a><br />
<strong>Mail:</strong> 55 City Centre Drive, Suite 800, Mississauga, Ontario L5B 1M3</p>
<h2 style="margin-top: 50px;">Strollers, Wheelchairs and Power Scooters</h2>
<p>For your shopping convenience, Single and Double Strollers are available at Guest Services for a fully refundable $20 deposit per device, cash only please. 2 hour limit per rental.  </p>
<p>Do you need some assistance getting around? Then visit the Guest Services to rent a wheelchair or electric scooter during your visit to Northgate. Wheel chairs and power scooters require a $20 cash only fully refundable deposit per device, with a 2 hour limit per rental.
</p></div>
<section class="clear fix" style="background: black; padding: 2em 15px; margin: 0px -15px;">
<div id="gift-cards-div" class="border-bottom">
<h2 id="gift-cards">Gift Cards</h2>
<p style="color: white;"><strong>Available at Guest Services.</strong> Take the guesswork out of finding that perfect gift for that special person. For any occasion, all year round you can give the gift of Northgate with our Gift Cards. Northgate Gift Cards are available in any denomination of Mall Money up to $500, and can be used at all participating Northgate Merchants. This gift card is issued by PACE Savings &amp; Credit Union Limited.</p>
</div>
<div id="benefits" class="clearfix">
<div class="left">
<h2 style="color: #f10044;">Corporate Benefits</h2>
<p style="color: white;">Northgate Gift Cards make for an excellent way for your business or organization to recognize individual staff achievements as well as larger group accolades. In addition, they make for dynamic workplace incentives. Call 705-472-8110 today to learn more about the benefits of corporate orders.</p>
</div>
<div class="right">
<h2 style="color: #f10044;">Participating Merchants</h2>
<p style="color: white;">Currently, over 90% of the stores in Northgate Shopping Centre accept Mall Money offered through the Gift Card, however, retailer participation is on a voluntary basis. Please inquire prior to making your purchase to ensure the store accepts Northgate Gift Cards. <a href="mailto:info@northgateshopping.com">info@northgateshopping.com</a></p>
</div>
</div>
</section>
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
</div>
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
</section>
	

                </div>

            
        
    				
    				
    				
    				    
    				</div>
    			</div>
			</div>
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
