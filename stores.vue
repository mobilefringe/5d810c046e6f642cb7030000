<template>
    <div> <!-- without an outer container div this component template will not render -->
        <loader v-if="!dataLoaded"></loader>
        <transition name="fade">
            <div v-if="dataLoaded" v-cloak>
        		<div class="page_header" v-if="pageBanner" v-bind:style="{ backgroundImage: 'url(' + pageBanner.image_url + ')' }">
        			<div class="site_container">
        				<div class="header_content caps">
        					<h1>{{ $t("stores_page.directory") }}</h1>
        				</div>
        			</div>
        		</div>
        		<div class="site_container page_content">
        			<div class="stores_header_container">
        				<div class="stores_button_container search position_relative">
        					<search-component :list="allStores" :placeholder="$t('stores_page.find_your_store')" suggestion-attribute="name" v-model="search_result" @select="onOptionSelect" class="text-left">
        						<template slot="item" scope="option" class="manual">
        							<article class="media">
        								<p>{{ option.data.name }}</p>
        							</article>
        						</template>
        					</search-component>
        					<i id="search_icon" class="fa fa-search"></i>
        				</div>
        				
        				<div class="store_category stores_button_container">
        					<v-select 
        					    v-model="selectedCat" 
        					    :options="dropDownCats" 
        					    :searchable="false" 
        					    :on-change="filterByCategory" 
        					    class="category-select" 
        					    placeholder="Category" 
        					    id="selectByCat"
        					    transition="dropdown-fade"
        				    ></v-select>
        				</div>
        				
        				<router-link to="/map">
        				    <div class="stores_button_container map_link">
        				        {{ $t("stores_page.view_map") }}
        				    
        				    </div>
        				</router-link>
        			</div>
        			<div v-masonry transition-duration="0.5s" item-selector=".stores-grid-item" horizontal-order="true">
                        <transition-group name="custom-classes-transition" enter-active-class="animated fadeInUp" leave-active-class="animated fadeOut" tag="div">
                            <div v-masonry-tile  v-for="(store, index) in allStores" :key="index" class="stores-grid-item">
                        	    <div class="store_logo_container">
                        	        <router-link :to="'/stores/'+ store.slug">
                            			<div v-if="!store.no_store_logo">
                            			    <img class="transparent_logo" src="//codecloud.cdn.speedyrails.net/sites/5b1550796e6f641cab010000/image/png/1536094421888/default_background.png" alt="">
                            			    <img  class="store_img" :src="store.store_front_url_abs" alt="">
                            			</div>
                            			
                                        <div v-else class="no_logo_container">
                                            <img class="transparent_logo" src="//codecloud.cdn.speedyrails.net/sites/5b1550796e6f641cab010000/image/png/1536094421888/default_background.png" alt="">
                                            <div class="no_logo_text">
                                                <div class="store_text">
                                                    <h5>{{ store.name }}</h5>
                                                </div>
                                            </div>
                                        </div>
                            			<div class="store_tag" v-if="store.total_published_promos">
        									<div class="store_tag_text">{{ $t("stores_page.promotion") }}</div>
        								</div>
        								<div class="store_tag" v-if="!store.total_published_promos && store.is_coming_soon_store">
        									<div class="store_tag_text">{{ $t("stores_page.coming_soon") }}</div>
        								</div>
        								<div class="store_tag" v-if="!store.total_published_promos && !store.is_coming_soon_store && store.is_new_store">
        									<div class="store_tag_text">{{ $t("stores_page.new_store") }}</div>
        								</div>
        								<div class="store_details">
        								    <div class="store_text">
        								        <h5>{{ store.name }}</h5>
    								        </div>    
        								</div>
                            		</router-link>
                        	    </div>
                            </div>
                        </transition-group>
                    </div>
        		</div>
	        </div>
	    </transition>
	</div>
</template>

<script>
    define(["Vue", "vuex", "vue-select", "vue!search-component", "masonry", "vue-masonry-plugin"], function(Vue, Vuex, VueSelect, SearchComponent, masonry, VueMasonryPlugin) {
        Vue.use(VueMasonryPlugin.default);
        return Vue.component("stores-component", {
            template: template, // the variable template will be injected
            data: function() {
                return {
                    dataLoaded: false,
                    pageBanner : null,
                    search_result : null,
                    // storeFilter: 6439
                }
            },
            created (){
                this.loadData().then(response => {
                    var temp_repo = this.findRepoByName('Shop Banner');
                    if(temp_repo) {
                        try {
                            this.pageBanner = temp_repo.images[0];
                        } catch(e) {
                            
                        }
                    } else {
                        this.pageBanner = { "image_url": "https://via.placeholder.com/1920x300" }
                    }
                    
                    this.dataLoaded = true;
                    
                    this.query = this.$route.query.category
                    if(this.query == "dining_full_service"){
                      this.selectedCat = "Dining Full Service";
                      this.filterByCategory;
                    } else {
                        this.selectedCat = "All";
                        this.filteredStores = this.allStores;
                    }
                });
            },
            watch: {
                $route: function() {
                    this.query = this.$route.query.category
                    if(this.query == "dining_full_service"){
                      this.selectedCat = "Dining Full Service";
                      this.filterByCategory;
                    } else {
                        this.selectedCat = "All";
                        this.filteredStores = this.allStores;
                    }    
                },
                selectedCat: function() {
                    this.$nextTick(function() {
                        Vue.prototype.$redrawVueMasonry();
                    });    
                },
                // windowWidth: function() {
                //     if (this.windowWidth <= 768) {
                //         this.mobile_store = true;
                //     } else {
                //         this.mobile_store = false;
                //     }
                // }
            }
            computed: {
                ...Vuex.mapGetters([
                    'property',
                    'findRepoByName',
                    'processedStores',
                    
                    'processedCategories',
                    'storesByAlphaIndex',
                    'storesByCategoryName',
                    'findCategoryById',
                    'findCategoryByName'
                ]),
                allStores() {
                    var store_list = [];
                    var vm = this;
                    console.log(this.processedStores);
                    _.forEach(this.processedStores, function(value, key) {
                        // if(_.includes(value.categories, vm.storeFilter)) {
                            if (_.includes(value.image_url, 'missing')) {
                               value.no_store_logo = true;
                            } else {
                              value.no_store_logo = false;
                            }
                            store_list.push(value);
                        // }
                    });
                    return store_list
                }
                
                dropDownCats() {
                    var vm = this;
                    var cats = _.filter(this.processedCategories, function(o) { return _.toNumber(o.id) !== vm.dineFilter });
                    cats = _.map(cats, 'name');
                    cats.unshift('All');
                    return cats;
                },
                filterByCategory() {
                    category_id = this.selectedCat;
                    if (category_id == "All" || category_id == null || category_id == undefined) {
                        category_id = "All";
                    } else {
                        category_id = this.findCategoryByName(category_id).id;
                    }

                    if (category_id == "All") {
                        this.filteredStores = this.allStores;
                    } else {
                        var find = this.findCategoryById;
                        var filtered = _.filter(this.allStores, function(o) {
                            return _.indexOf(o.categories, _.toNumber(category_id)) > -1;
                        });
                        this.filteredStores = filtered;
                    }
                    var el = document.getElementById("selectByCat");
                    if(el) {
                        el.classList.remove("open");
                    }
                }
                
                
            },
            methods: {
                loadData: async function() {
                    try {
                        let results = await Promise.all([this.$store.dispatch("getData", "repos")]);
                    } catch (e) {
                        console.log("Error loading data: " + e.message);
                    }
                },
                onOptionSelect(option) {
                    this.search_result = "";
                    this.$router.push("/stores/" + option.slug);
                }
            }
        });
    });
</script>