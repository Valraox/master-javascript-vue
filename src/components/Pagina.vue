<template>
    <div class="general">
        <Slider texto="Página de pruebas" />
        <div class="center">
            <section id="content">
                <h2 class="subheader">Página de pruebas</h2>
                <h3>{{ id_desde_url }}</h3>
                <p>{{ nombre }}</p>
                <button @click="actualizarNombre()">Actualizar nombre</button>
                <button @click="redirigirAlBlog()">Redirigir al blog</button>
            </section>
            <Sidebar />
            <div class="clearfix"></div>
        </div>
    </div>
</template>

<script>
import Slider from "./Slider.vue";
import Sidebar from "./Sidebar.vue";
export default {
    name: "Pagina",
    components: {
        Slider,
        Sidebar,
    },
    created() {
        console.log("Componente creado");
    },
    mounted() {
        this.id_desde_url = this.$route.params.id;
        console.log("Componente montado");
    },
    updated() {
        console.log("Componente actualizado");
    },
    destroyed() {
        console.log("Componente destruido");
    },
    data() {
        return {
            id_desde_url: null,
            nombre: "Victor Robles",
        };
    },
    methods: {
        actualizarNombre() {
            this.nombre = "VICTOR ROBLES WEB";
        },
        redirigirAlBlog() {
            this.$router.push("/blog");
        },
    },
};
</script>