<template>
    <footer>
		<div class="site_container">
		    <div class="footer_logo">
		        <div class="footer_logo_container">
		            <div class="property_logo">
    			        <router-link to="/">
    			            <img src="//codecloud.cdn.speedyrails.net/sites/5d810c046e6f642cb7030000/image/png/1568743936000/Northgate-footer-logo.png" alt="Northgate Shopping Centre Logo."/>
    		            </router-link>
    		        </div>
    		        <div class="property_logo">
    			        
    			            <img src="//codecloud.cdn.speedyrails.net/sites/5d810c046e6f642cb7030000/image/png/1568328654000/Morguard.png" alt="Property managed by Morguard."/>
    		            
    		        </div>
    		        
		        </div> <!--footer_logo_container ends-->
		        
		        <div class="property_logo">
    			        
    			    <img src="//codecloud.cdn.speedyrails.net/sites/5d810c046e6f642cb7030000/image/png/1568828932000/safest-employer-award.png" alt="2018 Canada's Safest Employers Silver Winner recipient."/>
    		            
    		    </div>
		        
		        
		        <div class="center-block">
		            <nav id="footer_nav">
						<ul>
						    <li v-for="item in footer_menu_items" class="menu_item">
						        <router-link :to="item.href">{{$t(item.name)}}</router-link>
						    </li>
						</ul>
					</nav>    
		        </div>
		        <hr class="visible_phone">
		        
	            <div class="social_icons">
                    <span v-for="item in social_media">
                        <a :href="item.url" target="_blank">
                            <i :class="item.iconClass" aria-hidden="true"></i>
                            <p class="accessibility">{{ item.name }}</p>
                        </a>
                    </span>
                </div>    
             
		    </div>
			<div class="footer_terms">
				<p>&copy; {{copyright_year}} <span class="caps">{{property.name}}</span> | {{$t("footer.all_rights")}}<br class="visible_phone"/> <router-link to="/pages/southland-privacy-policy">{{$t("footer.privacy_policy")}}</router-link> | <span><a href="https://www.mallmaverick.com/system/site_images/photos/000/057/519/original/NorthgateCodeofConduct.pdf?1568825035" target="_blank">Code of Conduct</a></span>   |  <span> {{$t("footer.powered_by")}} <a href="https://www.mallmaverick.com" target="_blank">Mall Maverick</a></span></p>
				
				
				
				
			</div>
		</div>
    </footer>
</template>

<script>
    define(["Vue", "vuex", "moment", "moment-timezone", "vue-moment"], function (Vue, Vuex, moment, tz, VueMoment) {
        return Vue.component("footer-component", {
            template: template, // the variable template will be injected,
            data: function data() {
                return { }
            },
            props:['footer_menu_items', 'social_media'],
            computed: {
                ...Vuex.mapGetters([
                    'property',
                ]),
                copyright_year() {
                    return moment().year();
                }
            }
        });
    });
</script>