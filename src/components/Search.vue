<template>
    <div class="general">
        <Slider :texto="'Búsqueda: ' + searchString" />
        <div class="center">
            <section id="content">
                <h2 class="subheader" v-if="articles">Artículos encontrados</h2>
                <h2 class="subheader" v-else>Sin resultados para</h2>

                <div id="articles" v-if="articles">
                    <Articles :articles="articles" />
                </div>
                <div v-else>
                    <h2>No hay artículos que coincidan con tu búsqueda "{{ searchString }}"</h2>
                </div>
            </section>
            <Sidebar />
            <div class="clearfix"></div>
        </div>
    </div>
</template>

<script>
import axios from "axios";
import Global from "../Global";
import Slider from "./Slider.vue";
import Sidebar from "./Sidebar.vue";
import Articles from "./Articles.vue";
export default {
    name: "Search",
    components: {
        Slider,
        Sidebar,
        Articles,
    },
    mounted() {
        this.searchString = this.$route.params.searchString;
        this.getArticlesBySearch(this.searchString);
    },
    data() {
        return {
            url: Global.url,
            articles: null,
            searchString: null,
        };
    },
    methods: {
        getArticlesBySearch(searchString) {
            axios.get(this.url + "search/" + searchString).then((res) => {
                if (res.data.status == "success") {
                    this.articles = res.data.articles;
                }
                //console.log(this.articles);
            });
        },
    },
};
</script>