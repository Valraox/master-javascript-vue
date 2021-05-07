<template src="./CreateArticle.html"></template>

<script>
import axios from "axios";
import Global from "../Global";
import { required } from "vuelidate/lib/validators";
import swal from "sweetalert";
import Article from "../models/Article";
import Sidebar from "./Sidebar.vue";
export default {
    name: "EditArticle",
    components: {
        Sidebar,
    },
    data() {
        return {
            url: Global.url,
            file: "",
            article: new Article("", "", null, ""),
            submitted: false,
            isEdit: true,
        };
    },
    mounted() {
        //console.log(this.article);
        var articleId = this.$route.params.id;
        this.getArticle(articleId);
    },
    validations: {
        article: {
            title: { required },
            content: { required },
        },
    },
    methods: {
        getArticle(articleId) {
            axios.get(this.url + "article/" + articleId).then((res) => {
                if (res.data.status == "success") {
                    this.article = res.data.article;
                }
            });
        },
        fileChange() {
            this.file = this.$refs.file.files[0];
            //console.log(this.file);
        },
        save() {
            //console.log(this.article);
            this.submitted = true;
            var articleId = this.$route.params.id

            this.$v.$touch();
            if (this.$v.$invalid) {
                return false;
            } else {
                axios
                    .put(this.url + "article/" + articleId, this.article)
                    .then((res) => {
                        //console.log(res);
                        if (res.data.status == "success") {
                            // Subida de archivo
                            if (
                                this.file != null &&
                                this.file != undefined &&
                                this.file != ""
                            ) {
                                const formData = new FormData();
                                formData.append(
                                    "file0",
                                    this.file,
                                    this.file.name
                                );

                                var articleId = res.data.articleUpdated._id;
                                axios
                                    .post(
                                        this.url + "upload-image/" + articleId,
                                        formData
                                    )
                                    .then((res) => {
                                        if (res.data.article) {
                                            swal(
                                                "Artículo editado",
                                                "El artículo se ha editado correctamente",
                                                "success"
                                            );

                                            // Redirección a blog
                                            this.article = res.data.article;
                                            this.$router.push("/articulo/" + this.article._id);
                                        } else {
                                            // Mostrar alerta de error
                                            swal(
                                                "Edición fallida",
                                                "El artículo no se ha guardado correctamente",
                                                "error"
                                            );
                                        }
                                    })
                                    .catch((error) => {
                                        console.log(error);
                                    });
                            } else {
                                // Redirección a blog
                                swal(
                                    "Artículo editado",
                                    "El artículo se ha editado correctamente",
                                    "success"
                                );

                                this.article = res.data.articleUpdated;
                                this.$router.push("/articulo/" + this.article._id);
                            }
                        }
                    })
                    .catch((error) => {
                        console.log(error);
                    });
            }
        },
    },
};
</script>