<template>
    <div class="general">
        <Slider
            texto="Bienvenido al Curso de Vue con Víctor Robles de victorroblesweb.es"
            home="true"
        />
        <div class="center">
            <section id="content">
                <h2 class="subheader">Últimos artículos</h2>

                <!--Listado articulos-->
                <Articles :articles="articles" />
                <div id="articles"></div>
            </section>
            <Sidebar />
            <div class="clearfix"></div>
        </div>
    </div>
</template>

<script>
import axios from 'axios';
import Global from "../Global";
import Slider from "./Slider.vue";
import Sidebar from "./Sidebar.vue";
import Articles from "./Articles.vue";
export default {
    name: "LastArticles",
    components: {
        Slider,
        Sidebar,
        Articles,
    },
    mounted() {
        this.getLastArticles();
    },
    data() {
        return {
            url: Global.url,
            articles: null,
        };
    },
    methods: {
        getLastArticles() {
            axios.get(this.url + "articles/true").then((res) => {
                if (res.data.status == "success") {
                    this.articles = res.data.articles;
                }
                //console.log(this.articles);
            });
        },
    },
};
</script>