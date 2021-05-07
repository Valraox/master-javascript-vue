<template>
    <div class="general">
        <div class="center">
            <section id="content">
                <h2 class="subheader">Peliculas</h2>

                <div class="mis-datos" v-if="misDatos">
                    <p v-html="misDatos"></p>
                    <br>
                    {{ web | mayusculas | concatenaYear('Este es el mejor año') }}
                </div>

                <div class="favorita" v-if="favorita">
                    La película marcada es:
                    <h3>{{ favorita.title }}</h3>
                </div>

                <!--Listado articulos-->
                <div id="articles">
                    <div
                        v-for="pelicula in peliculasMayuscula"
                        v-bind:key="pelicula.title"
                    >
                        <Pelicula
                            :pelicula="pelicula"
                            @favorita="marcarFavorita"
                        ></Pelicula>
                    </div>
                </div>
            </section>
            <Sidebar />
            <div class="clearfix"></div>
        </div>
    </div>
</template>

<script>
import Pelicula from "./Pelicula.vue";
import Sidebar from "./Sidebar.vue";
export default {
    name: "Peliculas",
    components: {
        Pelicula,
        Sidebar,
    },
    data() {
        return {
            favorita: null,
            nombre: "Victor",
            apellidos: "Robles",
            web: "victorroblesweb.es/academy",
            peliculas: [
                {
                    title: "Batman vs Superman",
                    year: 2017,
                    image:
                        "https://www.ecestaticos.com/imagestatic/clipping/c81/cc8/c81cc8dd808a2deb28239b758bd9cdd2.jpg",
                },
                {
                    title: "Gran Torino",
                    year: 2015,
                    image:
                        "data:image/jpeg;base64,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",
                },
                {
                    title: "El Señor de los Anillos",
                    year: 2003,
                    image:
                        "https://sm.ign.com/t/ign_es/screenshot/default/lordoftherings-1564242962831_by2a.h720.jpg",
                },
                {
                    title: "Kill Bill Vol 1",
                    year: 2003,
                    image:
                        "https://isenacode.com/wp-content/uploads/2019/12/kill-bill-cabecera.jpg",
                },
            ],
        };
    },
    filters: {
        mayusculas(value){
            return value.toUpperCase();
        },
        concatenaYear(value, message){
            var date = new Date();
            return value + ' ' + date.getFullYear() + ' ' + message;
        }
    },
    computed: {
        peliculasMayuscula() {
            var peliculasMod = this.peliculas;
            for (var i = 0; i < peliculasMod.length; i++) {
                peliculasMod[i].title = peliculasMod[i].title.toUpperCase();
            }
            return peliculasMod;
        },
        misDatos() {
            return (
                this.nombre +
                " " +
                this.apellidos +
                "<br/>" +
                "<strong>Sitio web: </strong>" +
                this.web
            );
        },
    },
    methods: {
        marcarFavorita(favorita) {
            // console.log(favorita);
            // alert('Se ha ejecutado el evento en el padre');
            this.favorita = favorita;
        },
    },
};
</script>