<template>
    <div class="container pt-4 pb-4">
        <div class="container content">
            <h1 class="pb-4 text-center">
                Deadline Bersama
            </h1>
            <div class="col-md-12 mb-3 text-center">
                <btn-create @showModalCreate="showModalCreate"/>
            </div>
            <div class="row">
                <b-col lg="6 mb-4">
                    <b-form-group
                        label="Per page"
                        label-cols-sm="6"
                        label-cols-md="4"
                        label-cols-lg="2"
                        label-align-sm="left"
                        label-size="sm"
                        label-for="perPageSelect"
                        class="mb-0"
                        >
                        <b-form-select
                            v-model="perPage"
                            id="perPageSelect"
                            size="sm"
                            :options="pageOptions"
                        ></b-form-select>
                    </b-form-group>
                </b-col>
                <b-col lg="6 mb-4">
                    <b-form-group
                    label="Find"
                    label-cols-sm="3"
                    label-align-sm="right"
                    label-size="sm"
                    label-for="filterInput"
                    class="mb-0"
                    >
                        <b-input-group size="sm">
                            <b-form-input
                            v-model="filter"
                            type="search"
                            id="filterInput"
                            placeholder="Type to Search"
                            ></b-form-input>
                            <b-input-group-append>
                            <b-button :disabled="!filter" @click="filter = ''">Clear</b-button>
                            </b-input-group-append>
                        </b-input-group>
                    </b-form-group>
                </b-col>
                <div class="col-md-12">
                    <b-table id="table" responsive filter-debounce="500" striped :current-page="currentPage" :per-page="perPage" hover :items="items" :fields="fields" show-empty :filter="filter" @filtered="onFiltered" head-variant="dark"> 
                         <template v-slot:cell(due_date_time)="data">
                            {{ formatDate(data.item.due_date_time) }}
                        </template>
                        <template v-slot:cell(action)="data">
                            <btn-delete class="ml-1 mb-1" :id="data.item.id" @deleteData="deleteData(data.item.id)"/>
                            <btn-update class="ml-1 mb-1" :id="data.item.id" @showModalUpdate="showModalUpdate(data.item)"/>
                        </template>
                    </b-table>   
                </div>
                <div class="col-md-12">
                    <b-pagination
                        v-model="currentPage"
                        :total-rows="totalRows"
                        :per-page="perPage"
                        align="fill"
                        size="sm"
                        class="my-0"
                        ></b-pagination>
                </div>
                <b-modal ref="modal-update" title="Update Data" hide-footer>
                    <div>
                        <b-form @submit="updateData">

                            <b-form-group id="input-group-1" label="Nama" label-for="input-1">
                                <b-form-input
                                    id="input-1"
                                    v-model="updateFields.name"
                                    required
                                ></b-form-input>
                            </b-form-group>

                            <b-form-group id="input-group-2" label="Matkul" label-for="input-2">
                                <b-form-input
                                    id="input-2"
                                    v-model="updateFields.matkul"
                                    required
                                ></b-form-input>
                            </b-form-group>

                            <b-form-group id="input-group-3" label="Deskripsi" label-for="input-3">
                                <b-form-input
                                    id="input-3"
                                    v-model="updateFields.description"
                                    required
                                ></b-form-input>
                            </b-form-group>

                            <b-form-group id="input-group-4" label="Tanggal Deadline" label-for="input-4">
                                <b-form-datepicker
                                    id="input-4"
                                    v-model="updateFields.dueDate"
                                    required
                                    ></b-form-datepicker>
                            </b-form-group>

                            <b-form-group id="input-group-5" label="Waktu Deadline" label-for="input-5">
                                <b-form-timepicker
                                    v-model="updateFields.dueTime"
                                    locale="en"
                                ></b-form-timepicker>
                            </b-form-group>

                            <b-button variant="primary" @click.prevent="updateData()">Update</b-button>
                        </b-form>
                        <!-- <b-card class="mt-3" header="Form Data Result">
                        <pre class="m-0">{{ updateFields }}</pre>
                        </b-card> -->
                    </div>
                </b-modal>
                <b-modal ref="modal-create" title="Create Data" hide-footer>
                    <div>
                        <b-form @submit="createData">

                            <b-form-group id="input-group-1" label="Nama" label-for="input-1">
                                <b-form-input
                                    id="input-1"
                                    v-model="createFields.name"
                                    required
                                ></b-form-input>
                            </b-form-group>

                            <b-form-group id="input-group-2" label="Matkul" label-for="input-2">
                                <b-form-input
                                    id="input-2"
                                    v-model="createFields.matkul"
                                    required
                                ></b-form-input>
                            </b-form-group>

                            <b-form-group id="input-group-3" label="Deskripsi" label-for="input-3">
                                <b-form-input
                                    id="input-3"
                                    v-model="createFields.description"
                                    required
                                ></b-form-input>
                            </b-form-group>

                            <b-form-group id="input-group-4" label="Tanggal Deadline" label-for="input-4">
                                <b-form-datepicker
                                    id="input-4"
                                    v-model="createFields.dueDate"
                                    required
                                    ></b-form-datepicker>
                            </b-form-group>

                            <b-form-group id="input-group-5" label="Waktu Deadline" label-for="input-5">
                                <b-form-timepicker
                                    v-model="createFields.dueTime"
                                    locale="en"
                                ></b-form-timepicker>
                            </b-form-group>

                            <b-button variant="primary" @click.prevent="createData()">Create</b-button>
                        </b-form>
                        <!-- <b-card class="mt-3" header="Form Data Result">
                        <pre class="m-0">{{ createFields }}</pre>
                        </b-card> -->
                    </div>
                </b-modal>
            </div>
        </div>
    </div>
</template>

<script>

import axios from 'axios'
import moment from 'moment'
import ButtonDelete from '@/components/ButtonDelete'
import ButtonUpdate from '@/components/ButtonUpdate.vue'
import ButtonCreate from './components/ButtonCreate.vue'

export default {
    name: "App",
    components: {
        'btn-delete': ButtonDelete,
        'btn-update': ButtonUpdate,
        'btn-create': ButtonCreate
    },
    mounted() {
        this.readData()
    },
    data() {
        return {
            fields: [
                {key: 'id', label: "ID", sortable: true},
                {key: 'name', label: "Nama", sortable: true},
                {key: 'matkul', label: "Matkul", sortable: true},
                {key: 'description', label: "Deskripsi", sortable: true},
                {key: 'due_date_time', label: 'Deadline', sortable: true },
                {key: 'action', label: ''}
            ],
            updateFields: {
                id: null,
                name: null,
                matkul: null,
                description: null,
                dueDate: null,
                dueTime: null
            },
            createFields: {
                name: null,
                matkul: null,
                description: null,
                dueDate: null,
                dueTime: null
            },
            items: [],
            filter: null,
            currentPage: 1,
            perPage: 10,
            pageOptions: [5, 10, 30],
            totalRows: 1,
        }
    },
    computed: {
        
    },
    methods: {
        readData() {
            this.items = [
                {
                    'id': 1,
                    'name': 'Proyek Akhir',
                    'matkul': 'TKAI',
                    'description': 'Video demo dan presentasi',
                    'due_date_time': new Date("2020-12-23T10:00:00Z")
                },
                {
                    'id': 2,
                    'name': 'Proyek Akhir',
                    'matkul': 'TKAI',
                    'description': 'Video demo dan presentasi',
                    'due_date_time': new Date("2020-12-23T10:00:00Z")
                },
                {
                    'id': 3,
                    'name': 'Proyek Awal',
                    'matkul': 'TKAI',
                    'description': 'Video demo dan presentasi',
                    'due_date_time': new Date("2020-12-23T10:00:00Z")
                },
                {
                    'id': 4,
                    'name': 'Proyek Akhir',
                    'matkul': 'TKAI',
                    'description': 'Video demo dan presentasi',
                    'due_date_time': new Date("2020-12-23T10:00:00Z")
                },
                {
                    'id': 5,
                    'name': 'Proyek Akhir',
                    'matkul': 'TKAI',
                    'description': 'Video demo dan presentasi',
                    'due_date_time': new Date("2020-12-23T10:00:00Z")
                },
            ]
        },
        deleteData(id) {
            this.items = this.items.filter(data => {
                return data.id != id
            });
            this.totalRows = this.items.length
        },
        async updateData() {
            await axios.put(
                "http://updatedeadlineLB-1219689781.us-east-1.elb.amazonaws.com/deadline/update",
                {
                    id: this.updateFields.id,
                    name: this.updateFields.name,
                    matkul: this.updateFields.matkul,
                    description: this.updateFields.description,
                    due: this.getISODateFormat(this.updateFields.dueDate, this.updateFields.dueTime)
                },
                {
                    headers: {
                         'Content-Type': 'application/json'
                    }
                }
                
            ).then(response => {
                console.log(response)
                location.reload()
            })
        },
        async createData() {
            var body = {
                    name: this.createFields.name,
                    matkul: this.createFields.matkul,
                    description: this.createFields.description,
                    due: this.getISODateFormat(this.createFields.dueDate, this.createFields.dueTime)
                }
            await axios.post(
                "http://reminder-me-create-deadline.us-east-1.elasticbeanstalk.com/deadline/create",
                body
                
            ).then(response => {
                console.log(response)
                location.reload()
            })
        },
        onFiltered(filteredItems) {
            this.totalRows = filteredItems.length
            this.currentPage = 1
        },
        formatDate(date) {
            return moment(String(date)).format('L LT')
        },
        showModalUpdate(data) {
            this.$refs['modal-update'].show()
            this.updateFields.id = data.id
            this.updateFields.name = data.name
            this.updateFields.matkul = data.matkul
            this.updateFields.description = data.description
            this.updateFields.dueDate = this.getDate(data.due_date_time)
            this.updateFields.dueTime = this.getTime(data.due_date_time)
        },
        showModalCreate() {
            this.$refs['modal-create'].show()
        },
        getDate(d) {
            return d.getUTCFullYear() + "-" + d.getMonth() + "-" + d.getDate()
        },
        getTime(d) {
            return d.getHours() + ":" + d.getMinutes() + ":" + d.getSeconds()
        },
        getISODateFormat(date, time) {
            return new Date(date + "T" + time + "Z")
        }
    }
}
</script>

<style>
    @import url('https://fonts.googleapis.com/css2?family=Poppins:wght@400;600&display=swap');
    h1 {
        font-family: 'Poppins', sans-serif;
        font-weight: 600 !important;
    }
    .page-link {
        color: #323F75 !important;
    }

    .table .thead-dark th, .page-item.active .page-link, #btn-modal {
        background-color: #323F75 !important;
        color: white !important;
        border-color: #323F75 !important;
    }

    .table-striped tbody tr:nth-of-type(even) {
        background-color: #FAFAFC;
    }

    .table-striped tbody tr:nth-of-type(even):hover {
        background-color: #FAFAFC;
    }

    .table-striped tbody tr:nth-of-type(odd) {
        background-color: white !important;
    }

    .table-striped tbody tr:nth-of-type(odd):hover {
        background-color: #FAFAFC !important;
    }

    .content {
        background-color: white;
        box-shadow: 0px 4px 8px rgba(0, 0, 0, 0.25);
        border-radius: 10px;
        padding: 80px 50px 80px 50px !important;
    }

</style>