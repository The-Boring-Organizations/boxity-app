<template>
    <div>
        <div class="row mt-4">
            <div class="col-12">
                <div class="breadcrumb-main">
                    <h2 class="text-capitalize fw-700 breadcrumb-title">
                        New Blog
                    </h2>
                </div>
            </div>
        </div>
        <form @submit.prevent="submitHandle">
            <div class="row">
                <div class="col-lg-8">
                    <div class="card card-Vertical card-default card-md mb-4">
                        <div class="card-body pb-md-30">
                            <div class="Vertical-form">
                                <div class="form-group">
                                    <div class="form-row">
                                        <div class="col-lg-12">
                                            <h6>Blog post title</h6>
                                            <input type="text" v-model="blog.title" placeholder="Enter title here"
                                                class="form-control mt-2">
                                        </div>
                                    </div>
                                </div>
                                <div class="form-group">
                                    <div class="form-row">
                                        <div class="col-lg-12">
                                            <h6 class="mb-2">Blog article</h6>
                                            <editor placeholder="Blog article write here..." v-model="blog.description"
                                                api-key="8ll77vzod9z7cah153mxwug6wu868fhxsr291kw3tqtbu9om" :init="{
                                                                height: 600,
                                                                menubar: true,
                                                                branding: false,
                                                                plugins: 'advlist autolink lists link image charmap preview anchor pagebreak code',
                                                                toolbar:
                                                                    'undo redo | fontselect | formatselect | bold italic backcolor | alignleft aligncenter alignright alignjustify | bullist numlist outdent indent | removeformat | checklist | code | emoticons | media | insertfile'
                                                        }" />
                                        </div>
                                    </div>
                                </div>
                                <hr class="my-4">
                                <h3>SEO Configurations:</h3>
                                <div class="form-group">
                                    <div class="form-row">
                                        <div class="col-lg-12">
                                            <h6>Title tag <span>(Optional)</span></h6>
                                            <input type="text" v-model="blog.seo_title" class="form-control mt-2">
                                            <small id="blog_seo_title_help" class="form-text text-muted">Enter a value
                                                for the &lt;title&gt; tag. If nothing is provided here we will just use
                                                the normal post title from above (optional)</small>
                                        </div>
                                    </div>
                                </div>
                                <div class="form-group">
                                    <div class="form-row">
                                        <div class="col-lg-12">
                                            <h6>Descriptions tag <span>(Optional)</span></h6>
                                            <textarea v-model="blog.seo_description" class="form-control mt-2" rows="3"
                                                cols="10"></textarea>
                                            <small id="blog_meta_desc_help" class="form-text text-muted">Meta
                                                description is better with 160 characters maximum (optional)</small>
                                        </div>
                                    </div>
                                </div>
                            </div>
                        </div>
                    </div>
                </div>
                <div class="col-lg-4">
                    <div class="card card-Vertical card-default card-md mb-4">
                        <div class="card-body pb-md-30">
                            <div class="Vertical-form">
                                <div class="form-group my-2">
                                    <h4>Published?</h4>
                                    <select class="form-control form-control-default my-1" v-model="blog.type">
                                        <option value="draft">
                                            Draft
                                        </option>
                                        <option value="published">
                                            Published
                                        </option>
                                    </select>
                                    <small id="blog_is_published_help" class="form-text text-muted">Should this be
                                        published? If not, then it
                                        won't be
                                        publicly viewable.
                                    </small>
                                </div>
                            </div>
                        </div>
                    </div>
                    <div class="card card-Vertical card-default card-md mb-4">
                        <div class="card-body pb-md-30">
                            <div class="Vertical-form">
                                <div class="form-group my-2">
                                    <h4>Featured Image</h4>
                                    <vue-dropzone useCustomSlot ref="document-upload" id="dropzone"
                                        :options="dropzoneOptions" class="dropzone mt-2">
                                        <div class="dropzone-custom-content">
                                            <h3 class="dropzone-custom-title">Drag and drop to upload
                                                attachment!</h3>
                                            <div class="subtitle">...or click to select a file from your
                                                computer</div>
                                        </div>
                                    </vue-dropzone>
                                    <small class="form-text text-muted">
                                        By default it will resize for all images based on the first image.</small>
                                </div>
                            </div>
                        </div>
                    </div>
                    <div class="card card-Vertical card-default card-md mb-4">
                        <div class="card-body pb-md-30">
                            <div class="Vertical-form">
                                <div class="form-group my-2">
                                    <h4>Categories</h4>
                                    <select class="form-control form-control-default my-1" v-model="blog.category">
                                        <option value="">Select categories:</option>
                                        <option v-for="categoriesGet in categoriesGet" :key="categoriesGet.id"
                                            v-bind:value="categoriesGet.categories_name">
                                            {{categoriesGet.categories_name}}
                                        </option>
                                    </select>
                                    <!-- Modal -->
                                    <div class="modal fade new-member" id="newCategory" role="dialog" tabindex="-1"
                                        aria-labelledby="staticBackdropLabel" aria-hidden="true">
                                        <div class="modal-dialog modal-dialog-centered">
                                            <div class="modal-content modal-lg radius-xl">
                                                <div class="modal-header">
                                                    <h6 class="modal-title fw-500" id="staticBackdropLabel">New category
                                                    </h6>
                                                    <button type="button" class="close" data-dismiss="modal"
                                                        aria-label="Close">
                                                        <i class="fal fa-times"></i>
                                                    </button>
                                                </div>
                                                <div class="modal-body">
                                                    <div class="new-member-modal">
                                                        <form>
                                                            <div class="form-group mb-20">
                                                                <input type="text" v-model="categories.categories_name"
                                                                    class="form-control" placeholder="Categories name">
                                                            </div>
                                                            <div class="form-group mb-20">
                                                                <editor placeholder="Category descriptions..."
                                                                    v-model="categories.description"
                                                                    api-key="8ll77vzod9z7cah153mxwug6wu868fhxsr291kw3tqtbu9om"
                                                                    :init="{
                                                                height: 300,
                                                                menubar: true,
                                                                branding: false,
                                                                plugins: 'advlist autolink lists link image charmap preview anchor pagebreak code',
                                                                toolbar:
                                                                    'undo redo | fontselect | formatselect | bold italic backcolor | alignleft aligncenter alignright alignjustify | bullist numlist outdent indent | removeformat | checklist | code | emoticons | media | insertfile'
                                                        }" />
                                                            </div>
                                                            <div class="button-group d-flex pt-25">
                                                                <button v-on:click="handleSubmitNewCategories"
                                                                    class="btn btn-primary-boxity btn-default btn-squared text-capitalize"
                                                                    data-dismiss="modal">submit
                                                                </button>
                                                                <button class="btn btn-light btn-default btn-squared fw-400
                                                        text-capitalize b-light color-light" data-dismiss="modal"
                                                                    aria-label="Close">cancel
                                                                </button>
                                                            </div>
                                                        </form>
                                                    </div>
                                                </div>
                                            </div>
                                        </div>
                                    </div>
                                    <!-- Modal -->
                                    <hr style="margin-top: 0; margin-bottom: 10px;">
                                    <a href="#" data-toggle="modal" data-target="#newCategory">+ New Category</a>
                                </div>
                            </div>
                        </div>
                    </div>
                    <div class="Vertical-form">
                        <div class="form-group my-2">
                            <div class="justify-content-end">
                                <button type="submit"
                                    class="btn btn-primary-boxity btn-default btn-block btn-squared px-30"
                                    data-dismiss="modal">Publish</button>
                            </div>
                        </div>
                    </div>
                </div>
            </div>

        </form>
    </div>
</template>
<script>
    import Swal from 'sweetalert2';
    import Editor from '@tinymce/tinymce-vue';
    import vue2Dropzone from 'vue2-dropzone';

    export default {
        components: {
            'editor': Editor,
            vueDropzone: vue2Dropzone,
        },
        title() {
            return 'New Blog';
        },
        data() {
            return {
                blog: {},
                categories: {},
                categoriesArr: [],
                categoriesGet: [],

                // Image
                // TODO: callback to save the ids of the uploaded file
                dropzoneOptions: {
                    url: '/api/blogs/images',
                    thumbnailWidth: 200,
                    maxFilesize: 2, // MB
                    addRemoveLinks: true,
                    autoDiscover: false,
                    dictRemoveFile: 'REMOVE'
                },
            }
        },
        created() {
            this.getCategories();
        },
        methods: {
            async submitHandle(e) {
                // console.log(e);
                this.$isLoading(true);
                await axios.post('/api/blogs', {
                        title: this.blog.title,
                        description: this.blog.description,
                        category: this.blog.category,
                        type: this.blog.type,
                        seo_title: this.blog.seo_title,
                        seo_description: this.blog.seo_description,
                    }).then(response => {
                        document.getElementById('ding').play();
                        Swal.fire({
                            icon: 'success',
                            title: 'Congratulations',
                            text: 'Success Create New Blog',
                        });
                        this.$router.push('/blog-management');
                        this.blog = {};
                    })
                    .catch(error => {
                        this.$Progress.fail();
                        document.getElementById('failding').play();
                        Swal.fire({
                            icon: 'warning',
                            title: 'Something wrong.',
                            confirmButtonText: `Ok`,
                            html: `There is something wrong on my side. Please click ok to refresh this page and see what is it. If
                it still exist, you can contact our developer. <br><br>Error message: ` +
                                error,
                        }).then((result) => {
                            if (result.isConfirmed) {
                                location.reload();
                            }
                        });
                    });
                this.$isLoading(false);
                console.log(this.blog);
            },
            async handleSubmitNewCategories() {
                // this.$Progress.start();
                this.$isLoading(true);
                await axios.post('/api/categories', this.categories).then(response => {
                    document.getElementById('ding').play();
                    Swal.fire({
                        icon: 'success',
                        title: 'Congratulations',
                        text: 'Success Create New Category',
                    });
                    this.categories = {};
                    this.getCategories();
                }).catch(error => {
                    this.$Progress.fail();
                    document.getElementById('failding').play();
                    Swal.fire({
                        icon: 'warning',
                        title: 'Something wrong.',
                        confirmButtonText: `Ok`,
                        html: `There is something wrong on my side. Please click ok to refresh this page and see what is it. If
            it still exist, you can contact our developer. <br><br>Error message: ` +
                            error,
                    });
                });
                // this.$Progress.finish();
                this.$isLoading(false);
            },
            async getCategories() {
                const resp = await axios.get('/api/categories');
                this.categoriesGet = resp.data;
            }
        },
    }

</script>
<style lang="css">
    .form-group {
        margin-bottom: 0;
    }

</style>
