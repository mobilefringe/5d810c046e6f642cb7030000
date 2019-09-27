<template>
    <div> <!-- without an outer container div this component template will not render -->
        <loader v-if="!dataLoaded"></loader>
        <transition name="fade">
            <div v-if="dataLoaded" v-cloak>
        		<div class="page_header" v-if="pageBanner" v-bind:style="{ backgroundImage: 'url(' + pageBanner.image_url + ')' }">
        			<div class="site_container">
        				<div class="header_content caps">
        					<h1>Leasing</h1>
        				</div>
        			</div>
        		</div>
	
        		<div class="site_container inside_page_content page_content leasing-content">
        			<div class="row">
        			    <div class="col-md-12">
                            <div class="page_body description_text text_left" v-if="locale=='en-ca'" v-html="currentPage.body"></div>
                            <div class="page_body description_text text_left" v-else v-html="currentPage.body_2"></div>
        			    </div>
        			</div>
        			<div class="row padding_tb_50" v-if="subPages.length > 0">
                        <div class="col-md-12">
                            <b-card v-for="(item, index) in subPages" :key="index" no-body class="mb-1">
                                <b-card-header header-tag="header" class="p-1" role="tab">
                                    <b-button block @click="item.show_tab = !item.show_tab; toggleOpen=!toggleOpen;" :class="item.show_tab ? 'collapsed' : null" :aria-controls="'tab-' + index" :aria-expanded="item.show_tab ? 'true' : 'false'" variant="info">
                                        {{ item.title }}
                                        <i v-if="item.show_tab"  class="fa fa-minus"></i>
                                        <i v-else  class="fa fa-plus"></i>
                                    </b-button>
                                </b-card-header>
                                <b-collapse v-model="item.show_tab" :id="'tab-' + index" :visible="item.show_tab" accordion="my-accordion" role="tabpanel" class="accordion_body">
                                    <b-card-body>
                                        <b-card-text>
                                            <div class="accordian-container" v-html="item.body"></div>
                                        </b-card-text>
                                    </b-card-body>
                                </b-collapse>
                            </b-card>
                        </div>
                    </div>  
                </div>
                <span style="visibility: hidden;width: 0;height: 0;display: inline-block;">{{ toggleOpen }}</span>
            </div>
        </transition>
    </div>
</template>
<style>
    .page_title {
        /*border-top:1px solid #aea99e;*/
        border-bottom:1px solid #aea99e;
        height: 35px;
        line-height: 35px;
    }
    #pages_container img{
        width: 100%;
        height: auto;
    }
    .acc_title {
        margin-top:20px;
    }
    img {
        max-width: 100%;
    }
</style>
<script>
    define(["Vue", "vuex"], function(Vue, Vuex) {
        return Vue.component("page-details-component", {
            template: template, // the variable template will be injected,
            data: function() {
                return {
                    dataLoaded: false, 
                    pageBanner : null,
                    currentPage: null,
                    subPages: {},
                    toggleOpen: false,
                }
            },
            props:['id', 'locale'],
            beforeRouteUpdate(to, from, next) {
                next();
                this.updatePageData(to.params.id);
            },
            created(){
               this.loadData().then(response => {
                    var temp_repo = this.findRepoByName('Leasing Banner');
                    if (temp_repo) {
                        try {
                            this.pageBanner = temp_repo.images[0];
                        } catch(e) {
                            
                        }
                    } else {
                        this.pageBanner = { "image_url": "" }
                    }

                    this.currentPage = response[0].data;
                    if (response[0].data.subpages) {
                        this.subPages = response[0].data.subpages;
                    }
                    
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
                            this.$store.dispatch('LOAD_PAGE_DATA', { url: this.property.mm_host + "/pages/northgate-leasing.json" }),
                            this.$store.dispatch("getData", "repos")]);
                        return results;
                    } catch (e) {
                        console.log("Error loading data: " + e.message);
                    }
                },
                updatePageData (id) {
                    this.$nextTick(function() {
                        this.loadData(id).then(response => {
                            if (response == null || response == undefined) {
                                this.$router.replace('/');
                            } else {
                                this.currentPage = response[0].data;
                                if (response[0].data.subpages) {
                                    this.subPages = response[0].data.subpages;
                                }
                            }
                        });    
                    });
                }
            }
        });
    });
</script>