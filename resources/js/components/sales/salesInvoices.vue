<template>
    <div>
        <div class="row mt-4">
            <div class="col-lg-12">
                <div class="breadcrumb-main">
                    <h2 class="text-capitalize fw-700 breadcrumb-title">Sales Invoices<br></h2>
                    <div class="breadcrumb-action justify-content-center flex-wrap" v-if="permissions.includes('CreateSalesInvoice')">
                        <div class="action-btn">
                            <router-link to="/sales/invoices/add" class="btn btn-sm btn-primary-boxity btn-add">
                                <i class="las la-plus fs-16"></i>New Sales Invoices</router-link>
                        </div>
                    </div>
                </div>
            </div>
            <div class="col-lg-12">
                <div class="card mb-3">
                    <div class="card-body">
                        <div class="userDatatable projectDatatable project-table bg-white border-0">
                            <div class="table-responsive">
                                <v-card-title>
                                    <v-text-field v-model="search" append-icon="mdi-magnify" label="Search here..."
                                        single-line hide-details></v-text-field>
                                </v-card-title>
                                <v-data-table :headers="headers" multi-sort :items="salesInvoiceData"
                                    :items-per-page="10" class="elevation-1" group-by="customers.company_name">
                                    <template v-slot:item.status="{item}">
                                        <span class="transparency-primary-boxity" v-if="item.status==0">
                                            Phase 1 passed
                                        </span>
                                        <span class="transparency-primary-boxity" v-else-if="item.status==1">
                                            Phase 2 passed
                                        </span>
                                        <span class="transparency-primary-boxity" v-else-if="item.status==2">
                                            <em class="fad fa-check"></em> All passed
                                        </span>
                                    </template>
                                    <template v-slot:item.actions="{item}">
                                        <a :href="`/report/sales/invoices/${item.si_number}`" target="_blank"
                                            class="view">
                                            <i class="fad fa-print"></i></a>
                                        <router-link :to="`/detail/sales/invoices/${item.si_number}`" class="edit">
                                            <i class="fad fa-eye"></i></router-link>
                                        <!-- <a v-on:click="deleteSalesInvoiceData(item.id)" class="remove">
                                            <i class="fad fa-trash"></i></a> -->
                                    </template>
                                </v-data-table>
                            </div>
                        </div>
                    </div>
                </div>
            </div>
        </div>
        <div class="row">
            <div class="col-xxl-12 col-lg-12 col-md-12 mb-25">
                <div class="feature-cards5 d-flex justify-content-between border-0 radius-xl bg-white p-25">
                    <div class="application-task d-flex align-items-center">
                        <div class="application-task-content">
                            <span><strong>Need help?</strong> Check out our <a href="https://help.boxity.id"
                                    target="_blank">documentation</a> or our <a href="#">getting
                                    started video series</a> to quickly learn the basics or <router-link
                                    to="/new-issue">reach out to our
                                    team</router-link>, we'd happy to help.</span>
                        </div>
                    </div>
                </div>
            </div>
        </div>
    </div>
</template>
<script>
    import Swal from 'sweetalert2';
    import Editor from '@tinymce/tinymce-vue';

    export default {
        components: {
            'editor': Editor
        },
        title() {
            return 'Sales Invoices';
        },
        data() {
            return {
                // datatable
                item: [],
                search: '',
                key: 1,
                salesInvoiceData: [],
                headers: [{
                    text: 'SI #',
                    value: 'si_number'
                }, {
                    text: 'Invoice Date',
                    value: 'invoice_date'
                }, {
                    text: 'Customer',
                    value: 'customers.company_name'
                }, {
                    text: 'Status',
                    value: 'status'
                }, {
                    text: 'Actions',
                    value: 'actions',
                    align: 'right',
                    filterable: false,
                    sortable: false
                }],
                // end datatable
                countItems: '0',
                permissions: []
            }
        },
        beforeMount(){
            this.permissions = this.$store.getters.getPermissions;
        },
        created() {
            this.loadItem();
        },
        methods: {
            async loadItem() {
                // this.$Progress.start();
                this.$isLoading(true);
                const resp = await axios.get('/api/sales/invoices');
                this.salesInvoiceData = resp.data;
                const count = await axios.get('/api/count-sales-invoice');
                this.countItems = count.data;
                // this.$Progress.finish();
                this.$isLoading(false);
            },
            async deleteSalesInvoiceData(id) {
                document.getElementById('failding').play();
                const result = await Swal.fire({
                    title: 'Delete data sales invoices?',
                    showCancelButton: true,
                    cancelButtonColor: '#d33',
                    confirmButtonText: `Delete`,
                });
                if (result.isConfirmed) {
                    await axios.delete('/api/sales-invoices/' + id);
                    this.loadItem();
                    document.getElementById('ding').play();
                    await Swal.fire({
                        icon: 'success',
                        title: 'Successfully Deleted',
                        text: 'Success deleted current Sales Invoice.'
                    });
                }
            },
        },
    }

</script>
<style lang="css">
    .form-group {
        margin-bottom: 0;
    }

</style>
