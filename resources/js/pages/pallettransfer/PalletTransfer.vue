<template>
    <div class="col-md-12">
        <div class="panel">
            <div class="panel-heading">
                <router-link :to="{ name: 'pallettransfers.add' }"><v-btn>Add Pallet Transfer</v-btn></router-link>
                
            </div>
            <v-spacer />
                
            <div class="panel-body">
              
                <!-- TABLE UNTUK MENAMPILKAN LIST SJP -->
                <template>
                    <v-card>
                        <v-card-title>
                            Pallet Transfer
                            <v-spacer></v-spacer>
                            <v-btn>
                            <download-excel 
                            :data= "pallettransfers.data"
                            type="csv"
                            name="PalletTransfers.csv">
                                Download Data
                            </download-excel>
                        </v-btn>
                        <v-spacer></v-spacer>
                            <v-text-field
                            v-model="search"
                            append-icon="mdi-magnify"
                            label="Search"
                            single-line
                            hide-details
                            ></v-text-field>
                        </v-card-title>
                        <v-data-table
                        :headers="headers"
                        :items="pallettransfers.data"
                        :search="search"
                        loading 
                        loading-text="Loading... Please wait"
                        >       
                            <template v-slot:item.status="{ item }">
                                <v-chip class="label label-default" v-if="item.status == 0">SENDING</v-chip>
                                <v-chip class="label label-primary" v-if="item.status == 1">RECEIVED</v-chip>
                                <!-- <span class="label label-primary" v-if="item.status == 0">SENDING</span>
                                <span class="label label-success" v-else-if="item.status == 1">RECEIVED</span> -->
                            </template>
                            <template v-slot:item.receive="{ item }">
                                <router-link :to="{ name: 'pallettransfers.edit', params: {id: item.pallet_transfer_id} }" v-if="item.status == 0 && $can('update pallettransfers')" >
                                    <v-btn color="success" small>Receive</v-btn>
                                </router-link>                        
                            </template>
                            <template v-slot:item.delete="{ item }">
                                <v-btn v-if="$can('delete pallettransfers')" color="error" small @click="deletePalletTransfer(item.pallet_transfer_id)">Delete</v-btn>                         
                            </template>
                        </v-data-table>
                    </v-card>
                </template>
              	<!-- TABLE UNTUK MENAMPILKAN LIST CUSTOMER -->

               
                    <!-- <div class="col-md-6">
                        <p v-if="sjpstatuss.data"><i class="fa fa-bars"></i> {{ sjpstatuss.data.length }} item dari {{ sjpstatuss.meta.total }} total data</p>
                    </div>
                    <div class="col-md-6">
                        <div class="pull-right">
                            <b-pagination
                                v-model="page"
                                :total-rows="sjpstatuss.meta.total"
                                :per-page="sjpstatuss.meta.per_page"
                                aria-controls="sjpstatuss"
                                v-if="sjpstatuss.data && sjpstatuss.data.length > 0"
                                ></b-pagination>
                        </div>
                    </div> -->
                
            </div>
        </div>
    </div>
</template>

<script>
import { mapActions, mapState } from 'vuex'

export default {
    name: 'DataPalletTransfer',
    created() {
        this.getPalletTransfer() //LOAD DATA SJP KETIKA COMPONENT DI-LOAD
    },
    data() {
        return {
            //FIELD YANG AKAN DITAMPILKAN PADA TABLE DIATAS
            headers: [
                { value: 'dep_pool', text: 'Departure Pool' },
                { value: 'dest_pool', text: 'Destination Pool' },
                { value: 'sender_name', text: 'Sender User' },
                { value: 'receiver_name', text: 'Receiver User' },
                { value: 'tp_number', text: 'TP Number' },
                { value: 'status', text: 'Status' },
                { value: 'good_pallet', text: 'Good Pallet' },
                { value: 'tbr_pallet', text: 'TBR Pallet' },
                { value: 'transporter_name', text: 'Transporter Name' },
                { value: 'vehicle_number', text: 'Vehicle Number' },
                { value: 'driver_name', text: 'Driver Name' },
                { value: 'reason', text: 'Reason' },
                { value: 'note', text: 'Note' },
                { value: 'receive', text: 'Receive'},
                { value: 'delete', text: 'Delete'},
            ],
            search: '',
        }
    },
    computed: {
        ...mapState('pallettransfer', {
            pallettransfers: state => state.pallettransfers //MENGAMBIL DATA CUSTOMER DARI STATE CUSTOMER
        }),
        //MENGAMBIL DATA PAGE DARI STATE CUSTOMER
        page: {
            get() {
                return this.$store.state.pallettransfer.page
            },
            set(val) {
                this.$store.commit('pallettransfer/SET_PAGE', val)
            }
        }
    },
    watch: {
        page() {
            this.getPalletTransfer() //KETIKA VALUE PAGE TERJADI PERUBAHAN, MAKA REQUEST DATA BARU
        },
        search() {
            this.getPalletTransfer(this.search) //KETIKA VALUE SEARCH TERJADI PERUBAHAN, MAKA REQUEST DATA BARU
        }
    },
    methods: {
        ...mapActions('pallettransfer', ['getPalletTransfer', 'removePalletTransfer']), 
        //KETIKA TOMBOL HAPUS DITEKAN MAKA FUNGSI INI AKAN DIJALANKAN
        deletePalletTransfer(id) {
            this.$swal({
                title: 'Are you sure?',
                text: "This will delete record Permanently!",
                type: 'warning',
                showCancelButton: true,
                confirmButtonColor: '#3085d6',
                cancelButtonColor: '#d33',
                confirmButtonText: 'Yes!'
            }).then((result) => {
                if (result.value) {
                    this.removePalletTransfer(id) //JIKA SETUJU MAKA PERMINTAAN HAPUS AKAN DI EKSEKUSI
                }
            })
        }
    }
}
</script>