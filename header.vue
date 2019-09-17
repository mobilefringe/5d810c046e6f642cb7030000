<template>
    <header id="header" :class="{ 'nav-up': navUp }">
		<div class="site_container">
		    <!--<div class="google_translate_container visible_phone">-->
      <!--          <div id="google_translate_element_m"></div>-->
      <!--      </div>-->
            
			<div class="header_logo">
				<div class="hidden_phone">
				    <div class="social_icons">
                        <span v-for="item in social_media">
                            <a :href="item.url" target="_blank">
                                <p class="accessibility">{{ item.name }}</p>
                                <i :class="item.iconClass" aria-hidden="true"></i>
                            </a>
                        </span>
                    </div>
                    <div class="header_hours">
                        <p v-if="todaysHours && !todaysHours.is_closed">Open Now: {{ todaysHours.open_time | moment("h a", timezone) }} - {{ todaysHours.close_time | moment("h a", timezone) }}</p>
                        <p v-else>Closed</p>
                    </div>
				</div>
				<div class="mobile_header">
				    <div class="property_logo center-block">
						<router-link to="/">
						    <img src="//codecloud.cdn.speedyrails.net/sites/5d810c046e6f642cb7030000/image/svg+xml/1568636780000/Northgate-logo.svg" :alt="property.name + ' Logo'"/>
					    </router-link>
					</div>
			        <div id="menu-icon" @click="showMenu = !showMenu" :class="{ open: showMenu }" class="visible_phone hamburger">
        				<div class="hamburger__line">
        					<span class="hamburger__dots"></span>
        					<span class="hamburger__dots hamburger__dots--right"></span>
        					<span class="hamburger__dots"></span>
        				</div> 
        				<div class="hamburger__line">
        					<span class="hamburger__dots hamburger__dots--up"></span>
        					<span class="hamburger__dots"></span>
        					<span class="hamburger__dots hamburger__dots--down"></span>
        				</div>
        				<div class="hamburger__line">
        					<span class="hamburger__dots"></span>
        					<span class="hamburger__dots hamburger__dots--left"></span>
        					<span class="hamburger__dots"></span>
        				</div> 
        			</div>
				</div>
				<div class="hidden_phone">
				    <!--<div id="desktopHeader">-->
        <!--                <div id="g_translater" class="text-center">-->
        <!--                    <div id="google_translate_element"></div>-->
        <!--                </div>-->
        <!--            </div>-->
				    <div class="header_search_container">
					    <search-component v-if="headerReady" :list="searchList" placeholder="Search" :suggestion-attribute="suggestionAttribute" :keys="keys" v-model="search_result" @select="onOptionSelect" :autocomplete="false" :minMatchCharLength="3" :tokenize="true" class="text-left">
                            <template slot="item" scope="option" class="manual">
                                <article class="media">
                                    <p>{{ option.data.name }}</p>
                                </article>
                            </template>
                        </search-component>
                    </div>
				</div>
			</div>
			<div class="row">
			    <div class="col-md-2 hidden_phone"></div>
				<div class="col-sm-12 col-md-8">
					<nav id="primary_nav" class="hidden_phone">
						<ul>
						    <li class="menu_item" v-for="item in menu_items" :id="item.id">
						        <router-link v-if="item.sub_menu == undefined" :to="item.href">{{$t(item.name)}}</router-link>
						        <span v-if="item.sub_menu != undefined">{{$t(item.name)}}</span>
						        <ul v-if="item.sub_menu">
						            <li v-for="sub_menu in item.sub_menu" class="dropdown_item">
						                <router-link :to="sub_menu.href">{{$t(sub_menu.name)}}</router-link>
						            </li>
								</ul>
						    </li>
						</ul>
					</nav>
				</div>
				<div class="nav_container visible_phone">
				    <transition name="custom-classes-transition" enter-active-class="animated slideInRight" leave-active-class="animated slideOutRight">
					    <nav id="mobile_nav" v-show="showMenu" class="">
					        <hr class="menu_top_line">
					        <ul>
					            <li v-for="(item,key) in menu_items" class="menu_item">
							        <router-link :to="item.href" v-if="item.sub_menu == undefined">{{$t(item.name)}}</router-link>
							        <div v-else>
							            <b-card no-body class="mb-1">
                                            <b-card-header header-tag="header" class="p-1" role="tab">
                                                <b-btn block @click="item.show_sub_menu = !item.show_sub_menu" :class="item.show_sub_menu ? 'collapsed' : null" :aria-controls="$t(item.name)" :aria-expanded="item.show_sub_menu ? 'true' : 'false'">
                                                    {{$t(item.name)}}
                                                    <i v-if="item.show_sub_menu"  class="fa fa-minus"></i>
                                                    <i v-else  class="fa fa-plus"></i>
                                                </b-btn>
                                            </b-card-header>
                                            <b-collapse v-model="item.show_sub_menu" :id="$t(item.name)" :visible="item.show_sub_menu" :accordion="$t(item.name)" role="tabpanel" class="accordion_body">
                                                <b-card-body v-for="sub_menu in item.sub_menu">
                                                    <p class="card-text" @click="showMenu = !showMenu">
                                                        <router-link :to="sub_menu.href">{{$t(sub_menu.name)}}</router-link>
                                                    </p>
                                                </b-card-body>
                                            </b-collapse>
                                        </b-card>
							        </div>
							    </li>
					        </ul>
						    <div class="mobile_nav_content visible_phone">
                                <div class="mobile_address_container center">
                                    <hr>
                                    <a href="https://goo.gl/maps/RJ5dV8dxP1y" target="_blank">
                                        <p class="property_address">{{ property.address1 }},<br>{{ property.city }}, {{ property.province_state }}</p>
                                    </a>
                                    <a :href="'tel:' + property.contact_phone" >
                                        <h5>{{ property.contact_phone }}</h5>
                                    </a>
                                    <hr>
                                </div>
                                <div class="social_icons center">
                                    <span v-for="item in social_media">
                                        <a :href="item.url" target="_blank">
                                            <p class="accessibility">{{ item.name }}</p>
                                            <i :class="item.iconClass" aria-hidden="true"></i>
                                        </a>
                                    </span>
                                </div> 
							</div>
						</nav>
				    </transition>
				</div>
				<div class="col-md-2 hidden_phone"></div>
			</div>
		</div>
    </header>
</template>

<script>
var _extends=Object.assign||function(e){for(var t=1;t<arguments.length;t++){var o=arguments[t];for(var n in o)Object.prototype.hasOwnProperty.call(o,n)&&(e[n]=o[n])}return e};define(["Vue","vuex","moment","moment-timezone","vue_router","routes","bootstrap-vue","google-translate"],function(e,t,o,n,s,r,i){return e.use(i),e.component("header-component",{template:template,props:["menu_items","social_media"],data:function(){return{scrolled:!1,navUp:!1,search_result:null,suggestionAttribute:"name",keys:["name","description","tags","store.name"],headerReady:!1,showMenu:!1,showMobileMenu:!1,noScroll:!1,windowWidth:0,mobileScroll:!1,lastScrollTop:0,navUp:!1,m:!1}},beforeRouteUpdate:function(e,t,o){document.body.classList.remove("nav-up"),this.navUp=!1,o()},watch:{$route:function(){document.body.classList.remove("nav-up"),this.navUp=!1,this.windowWidth<=768&&(this.showMenu=!1),_.forEach(this.menu_items,function(e){e.show_sub_menu=!1})},showMenu:function(){1==this.showMenu?(document.body.classList.add("no_scroll"),document.body.classList.remove("nav-up"),this.navUp=!1):0==this.showMenu&&document.body.classList.remove("no_scroll")}},created:function(){var e=this;this.loadData().then(function(){e.headerReady=!0}),this.$nextTick(function(){window.addEventListener("resize",this.getWindowWidth),this.getWindowWidth()})},mounted:function(){var e=this;this.$nextTick(function(){e.googleTranslateInit()})},computed:_extends({},t.mapGetters(["property","timezone","getPropertyHours","getPropertyHolidayHours","getTodayHours","processedStores","processedEvents","processedPromos","processedJobs"]),{locale:{get:function(){return this.$store.state.locale},set:function(e){this.$store.commit("SET_LOCALE",{lang:e})}},todaysHours:function(){return this.getTodayHours},searchList:function(){var e=this,t=this.processedEvents,n=[];_.forEach(t,function(t){var s=o.tz(e.timezone).format(),r=o.tz(t.show_on_web_date,e.timezone).format();s>=r&&(t.is_store=_.includes(t.eventable_type,"Property")?!1:!0,n.push(t))}),t=n;var s=this.processedPromos,r=[];_.forEach(s,function(t){var n=o.tz(e.timezone).format(),s=o.tz(t.show_on_web_date,e.timezone).format();n>=s&&(t.is_store=_.includes(t.promotionable_type,"Property")?!1:!0,r.push(t))}),s=r;var i=this.processedJobs;_.forEach(i,function(e){e.is_store=_.includes(e.jobable_type,"Property")?!1:!0});var a=this.processedStores;_.forEach(a,function(e){e.is_store=!0});var l=_.union(a,t,s,i);return l}}),methods:{googleTranslateInit:function(){var e=this,t=setInterval(function(){null!=google.translate.TranslateElement&&(clearInterval(t),e.googleTranslateElement("google_translate_element"))},100)},googleTranslateElement:function(e){new google.translate.TranslateElement({pageLanguage:"en",includedLanguages:"en,es",layout:google.translate.TranslateElement.InlineLayout.SIMPLE,gaTrack:!0,gaId:"UA-115200481-13"},e);var t=this;setTimeout(function(){t.windowWidth<768&&(t.m=!0,$("#g_translater").detach().appendTo("#google_translate_element_m"))},1500)},loadData:function(){var e;return regeneratorRuntime.async(function(t){for(;;)switch(t.prev=t.next){case 0:return t.prev=0,t.next=3,regeneratorRuntime.awrap(Promise.all([this.$store.dispatch("getData","stores"),this.$store.dispatch("getData","events"),this.$store.dispatch("getData","promotions"),this.$store.dispatch("getData","jobs")]));case 3:e=t.sent,t.next=9;break;case 6:t.prev=6,t.t0=t["catch"](0),console.log("Error loading data: "+t.t0.message);case 9:case"end":return t.stop()}},null,this,[[0,6]])},changeLocale:function(e){this.locale=e},getWindowWidth:function(){this.windowWidth=window.innerWidth,this.windowWidth<768&&0==this.m?(m=!0,$("#g_translater").detach().appendTo("#google_translate_element_m")):this.windowWidth>=768&&1==this.m&&(m=!1,$("#g_translater").detach().prependTo("#desktopHeader"))},onOptionSelect:function(e){this.$router.push({name:"search-results",query:{searchQuery:this.search_result},params:{results:e}}),this.$nextTick(function(){this.search_result=""})},handleScroll:function(){this.scrolled=window.pageYOffset>100},mobileDidScrolled:function(){if(!this.showMenu){this.mobileScroll=window.pageYOffset>0;var e=this;e.mobileScroll&&(e.mobileHasScrolled(),e.mobileScroll=!1)}},mobileHasScrolled:function(){var e=document.getElementById("header").offsetHeight,t=$(window).scrollTop();t>this.lastScrollTop&&t>e?this.navUp=!0:t+$(window).height()<$(document).height()&&(this.navUp=!1),this.lastScrollTop=t}},beforeMount:function(){window.addEventListener("resize",this.getWindowWidth),window.addEventListener("scroll",this.handleScroll),window.addEventListener("scroll",this.mobileDidScrolled)},beforeDestroy:function(){window.addEventListener("resize",this.getWindowWidth),window.removeEventListener("scroll",this.handleScroll),window.removeEventListener("scroll",this.mobileDidScrolled)}})});
</script>