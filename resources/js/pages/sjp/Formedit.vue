<template>
    <div>
       <!-- <div class="form-group" :class="{ 'has-error': errors.sjp_id }">
            <label for="">ID</label>
            <input type="text" class="form-control" v-model="sjp.sjp_id" :readonly="$route.name == 'sjps.edit'">
            <p class="text-danger" v-if="errors.sjp_id">{{ errors.sjp_id[0] }}</p>
        </div> -->
        
        <!-- <div class="form-group" :class="{ 'has-error': errors.departure_pool_pallet_id }">
            <label for="">Departure</label>
            <input type="text" class="form-control" v-model="sjp.departure_pool_pallet_id">
            <p class="text-danger" v-if="errors.departure_pool_pallet_id">{{ errors.departure_pool_pallet_id[0] }}</p>
        </div> -->
        <!-- <div class="form-group">
            <label>Departure Pool Pallet </label>
            <select class='form-control' v-model='sjp.departure_pool_pallet_id'>
                <option value='0' >Select Departure</option>
                <option v-for='data in pools.data' :value='data.pool_pallet_id'>{{ data.pool_name }}</option>
            </select>
        </div> -->
        <!-- <div class="form-group" :class="{ 'has-error': errors.departure_pool_pallet_id }">
            <label for="">Departure Pool</label>
            <p v-for='data in pools.data' :value='data.pool_pallet_id'>{{ data.pool_name }}</p>
        </div> -->
        <!-- <div class="form-group">
            <label>Departure Pool Pallet</label>
            <select class='form-control' v-model='sjp.departure_pool_pallet_id'>
                <option value='0' >Select Departure</option>
                <option v-for='data in pools.data' :value='data.pool_pallet_id'>{{ data.pool_name }}</option>
            </select>
        </div> -->
       <p>SJP NUMBER: {{sjp.sjp_number}}</p>
        <div class="form-group">
                            <label>Change Vehicle Number</label>
                            <select class='form-control' v-model='sjp.vehicle_id'>
                              <option value='0' >Change Vehicle</option>
                              <option v-for='data in vehicles.data' :value='data.vehicle_id'>{{ data.vehicle_number }}</option>
                            </select>
                        </div>
       
        <div class="form-group">
                            <label>Change Driver Name</label>
                            <select class='form-control' v-model='sjp.driver_id'>
                              <option value='0' >Change Driver</option>
                              <option v-for='data in drivers.data' :value='data.driver_id'>{{ data.driver_name }}</option>
                            </select>
                        </div>
                    
        <!-- <div class="form-group" :class="{ 'has-error': errors.transporter_id }">
            <label for="">Transporter</label>
            <input type="text" class="form-control" v-model="sjp.transporter_id">
            <p class="text-danger" v-if="errors.transporter_id">{{ errors.transporter_id[0] }}</p>
        </div> -->
        <!-- <div class="form-group" :class="{ 'has-error': errors.sjp_number }">
            <label for="">SJP No</label>
            <input type="text" class="form-control" v-model="sjp.sjp_number">
            <p class="text-danger" v-if="errors.sjp_number">{{ errors.sjp_number[0] }}</p>
        </div> -->
       
    </div>
</template>

<script>
import { mapState, mapActions, mapMutations } from 'vuex'
export default {
    name: 'FormSjp',
    created() {
        this.getVehicleForm(),this.getPoolForm(),this.getDriverForm(),this.getTransporters() //LOAD DATA SJP KETIKA COMPONENT DI-LOAD
    },
    // created() {
    //     this.getPools() //LOAD DATA SJP KETIKA COMPONENT DI-LOAD
    // },
    computed: {
        ...mapState(['errors']), //LOAD STATE ERROR UNTUK DITAMPILKAN KETIKA TERJADI ERROR VALIDASI
        ...mapState('sjp', {
            sjp: state => state.sjp //LOAD DATA CUSTOMER DARI STATE CUSTOMER
        }),
        ...mapState('pool', {
            pools: state => state.pools //MENGAMBIL DATA CUSTOMER DARI STATE CUSTOMER
        }),
        ...mapState('vehicle', {
            vehicles: state => state.vehicles //MENGAMBIL DATA CUSTOMER DARI STATE CUSTOMER
        }),
        ...mapState('driver', {
            drivers: state => state.drivers //MENGAMBIL DATA CUSTOMER DARI STATE CUSTOMER
        }),
         ...mapState('transporter', {
            transporters: state => state.transporters //MENGAMBIL DATA CUSTOMER DARI STATE CUSTOMER
        }),
        ...mapState('user', {
            authenticated: state => state.authenticated
        }),
    },
    methods: {
        ...mapMutations('sjp', ['CLEAR_FORM']), 
        ...mapActions('pool', ['getPoolForm']),
        ...mapActions('vehicle', ['getVehicleForm']),
        ...mapActions('driver', ['getDriverForm']),
        ...mapActions('transporter', ['getTransporters']),
    },
    destroyed() {
        this.CLEAR_FORM() //KETIKA COMPONENT DITINGGALKAN, BERSIHKAN DATA
    },
}
</script>