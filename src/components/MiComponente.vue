<template>
    <section id="content">
        <h2 class="subheader">{{ titulo }}</h2>
        <hr />
        <h3>Este es MI COMPONENTE</h3>
        <h3>{{ subtitulo }}</h3>
        <hr />

        <h3>Reactividad</h3>
        <input type="text" v-model="nombre" />
        <p>
            Mi nombre es: <strong>{{ nombre }}</strong>
        </p>

        <h3>Directivas condicionales</h3>
        <input type="number" v-model="edad" />
        <p>
            Mi edad es: <strong>{{ edad }}</strong>
        </p>

        <p
            class="alert-success-provisional"
            v-if="edad && edad >= 18 && edad < 65"
        >
            Eres mayor de edad
        </p>
        <p class="alert-purple-provisional" v-else-if="edad && edad >= 65">
            Eres de avanzada edad
        </p>
        <p class="alert-danger-provisional" v-else>
            No eres mayor de edad, acceso restringido
        </p>

        <h3>Directiva iterativa</h3>
        <ol>
            <li v-for="pelicula in peliculas" :key="pelicula">
                {{ pelicula }}
            </li>
        </ol>
    </section>
</template>

<script>
export default {
    name: "MiComponente",
    data() {
        return {
            titulo: "Mi Componente",
            subtitulo: "Soy el el subtítulo de mi componente",
            nombre: "",
            edad: 18,
            peliculas: [
                "Batman vs Superman",
                "El señor de los anillos",
                "Spiderman",
                "Anabelle"
            ],
        };
    },
};
</script>