<template>
    <div class="wrapper__container">
        <div class="top-page">
            <div class="container">
                <div class="row">
                    <div class="col-6">
                        <h1 v-if="!this.edit_activity">
                            Новый вид деятельности
                        </h1>
                         <h1 v-if="this.edit_activity">
                            Редактирование
                        </h1>
                    </div>
                </div>
            </div>
        </div>

        <section class="section">
            <div class="container">
                <div class="row">
                    <div class="col-6">

                        <form action="">
                            <div class="list-block__section" v-if="!this.edit_activity">
                                <Activities
                                    @activity = "setActivity"
                                ></Activities>
                            </div>

                            <div class="list-block list-block--small list-block--block" >

                                <div class="list-block__item" v-if="select_script.includes('addres')">
                                    <div class="title">
                                        Адрес производства
                                    </div>
                                    <div class="list-block__title">
                                        Укажите месторасположение производства, либо ближайший населённый пункт:
                                    </div>
                                    <div class="form">
                                        <div class="form__row">
                                            <div class="form__field ">
                                                <div class="form__field-title">
                                                    Населённый пункт
                                                </div>
                                                <input type="text" :class="cityError" v-model="city" placeholder="Константиновский">
                                            </div>
                                        </div>
                                        <div class="form__row">
                                            <div class="form__field  form__field--secondary">
                                                <div class="form__field-title">
                                                    Улица
                                                </div>
                                                <input type="text" :class="streetError" v-model="street" placeholder="Например: Ленина">
                                            </div>
                                            <div class="form__field form__field--little">
                                                <div class="form__field-title">
                                                    Номер
                                                </div>
                                                <input type="text" :class="houseError" v-model="house" placeholder="20">
                                            </div>
                                        </div>
                                    </div>
                                </div>

                                <div class="list-block__item" v-if="select_script.includes('materials_checkbox')">
                                    <div class="title">
                                        Материалы 
                                        <span style="color: red" v-if="materialsShowError">Укажите хотябы один метериал</span>
                                    </div>
                                    <div class="list-block__title">
                                        Выберите материалы, которые производит или добывает ваша организация в указанном
                                        месте:
                                    </div>

                                    <Checkboxes 
                                        :items = "materials"
                                        :savedItem = "selectedMaterials"
                                        @newList = "setMaterials"
                                    > </Checkboxes>

                                </div>

                                <div class="list-block__item" v-if="select_script.includes('materials_select')">
                                    <div class="title">
                                        Материал
                                    </div>
                                    <div class="list-block__title">
                                        Укажите, чем торгует ваша организация:
                                    </div>
                                    <div class="form">
                                        <div class="form__row">
                                            <div class="form__field">
                                                <Select
                                                    :items="materials"
                                                    :selectedItem="selectedMaterials[0]"        
                                                    @selected="setMaterial"
                                                ></Select>                                  
                                            </div>
                                        </div>
                                    </div>
                                </div>

                                 <Vehicle
                                    v-if="select_script.includes('vehicle_select')"
                                    :savedVehicle="selectedVehicles"
                                    :savedCopacity="selectCopacities"
                                    :showErrors="showErrors"
                                    @setVehicle="setVehicles"
                                    @setCopacities = "setCopacities"
                                ></Vehicle>

                                <div class="list-block__item" v-if="select_script.includes('vehicle_types_checkbox')">
                                    <div class="title">
                                        Способы поставки
                                        <span style="color: red" v-if="vehiclesTypeShowError">Укажите способ поставки</span>
                                    </div>
                                    <div class="list-block__title">
                                        Возможные способы доставки или отгрузки с данного производства:
                                    </div>
                                     <Checkboxes 
                                        :items = "vehicle_types"
                                        :savedItem = "selectedVehiclesType"
                                        @newList = "setVehicleTypes"
                                    > </Checkboxes>
                                </div>

                                <div class="list-block__item" v-if="select_script.includes('branches_checkbox')">
                                    <div class="title">
                                        Отрасли строительства
                                        <span style="color: red" v-if="bildingBranchesShowError">Укажите отрасли строительства</span>
                                    </div>
                                    <div class="list-block__title">
                                        Укажите отрасли строительства, в которых ваша организация осуществляет свою
                                        деятельность:
                                    </div>

                                    <div class="checkbox__list">
                                        <Checkboxes v-if = "bilding_branches"
                                            :items = "bilding_branches"
                                            :savedItem = "select_bilding_branches"
                                            @newList = "set_bilding_branches"
                                        > </Checkboxes>
                                    </div>
                                </div>

                            </div>

                            <div class="list-block list-block--bottom">
                                <div class="list-block__item">
                                    <div class="title">
                                        действия
                                    </div>
                                    <div class="mark">
                                        <div class="mark__item">
                                            <input type="checkbox" v-model="isMain" name="mark">
                                            <div class="mark__icon">
                                                <svg width="28" height="20" viewBox="0 0 28 20" fill="none"
                                                    xmlns="http://www.w3.org/2000/svg">
                                                    <rect class="fill" y="8" width="28" height="4" rx="2"></rect>
                                                    <circle class="stroke" cx="18" cy="10" r="9.5" fill="white">
                                                    </circle>
                                                </svg>

                                            </div>
                                            <div class="mark__text">Основной вид деятельности</div>
                                        </div>

                                    </div>
                                </div>
                            </div>
                        </form>


                        <div class="btn-block btn-block--row btn-block--end">
                            <a @click="back" class="btn btn--cancel btn--middle m-order-2">Отменить</a>
                            <a @click="saveActivity" :disabled="!validation" class="btn  btn--middle btn--default m-order-1">Сохранить</a>
                        </div>
                    </div>
                </div>

            </div>
        </section>

    </div>

</template>

<script>

import eventHub from '../../eventHub'
import breadCrumbs from '../breadCrumbs/breadCrumbs'
import Activities from '../general/Activities.vue'
import Checkboxes from '../general/Checkboxes.vue'
import Select from '../general/Select.vue'
import Vehicle from '../general/Vehicle.vue';

const axios = require('axios').default;

export default {
    props: {
        edit_activity: {
            defalut: false
        }
    },
    components: {
        Activities, Checkboxes, Select, Vehicle
    },
    data: () => ({
        activities: [],
        selectActivity: false,
        materials: [],
        vehicle_types: [],
        selectedMaterials: [],
        selectedVehiclesType: [],
        vehicles: [],
        copacities: [],
        selectCopacities: [],
        selectedVehicles: [],
        bilding_branches: [],
        select_bilding_branches: [],
        city: '',
        street: '',
        house: '',
        isMain: false,
        select_script: [],
        showErrors: false,
        activities_scripts: {
            construction_organizations : ['branches_checkbox'],
            transport_organizations:     ['vehicle_select', 'copacity_checkbox'],
            trade_organizations:         ['vehicle_types_checkbox', 'materials_select'],
            building_materials:          ['vehicle_types_checkbox', 'materials_checkbox', 'addres'],
            aggregates_manufacturers:    ['vehicle_types_checkbox', 'materials_checkbox', 'addres']   
        },
        select_validation_sripts: [],
        validation_sripts: {
            construction_organizations : ['bildingBranchesValid'],
            transport_organizations:     ['copacitiesValid'],
            trade_organizations:         ['vehiclesTypeValid', 'materialsValid'],
            building_materials:          ['vehiclesTypeValid', 'materialsValid', 'cityValid', 'streetValid', 'houseValid'],
            aggregates_manufacturers:    ['vehiclesTypeValid', 'materialsValid', 'cityValid', 'streetValid', 'houseValid']   
        },
        activityData: {}
    }),
    methods: {
        setActivity(selectActivity){
            this.selectActivity = selectActivity;
            
        },
        back(){
            this.$emit('showProfile');
        },
        setMaterial(items){
            this.selectedMaterials = [items];
            this.activityData['materials'] = this.selectedMaterials;
        },
        setMaterials(items){
            this.selectedMaterials = items;
            this.activityData['materials'] = this.selectedMaterials;
        },
        setVehicleTypes(items){
            this.selectedVehiclesType = items;
            this.activityData['vehicle_types'] = this.selectedVehiclesType;
        },
        setVehicles(vehicles){
            this.selectedVehicles = [vehicles];
            this.activityData['vehicles'] = this.selectedVehicles;
        },
        setCopacities(copacities){
            this.selectCopacities = copacities;
            this.activityData['copacities'] = this.selectCopacities;
        },
        set_bilding_branches(selected){
            this.select_bilding_branches = selected;
            this.activityData['bilding_branches'] = this.select_bilding_branches;
        },
        saveActivity(){
        
            if(!this.formIsValid()){
                this.showErrors = true;
                document.body.scrollIntoView({behavior: 'smooth'});
                return;
            }
            if(this.select_script.includes('addres')){
                this.activityData['city'] = this.city;
                this.activityData['street'] = this.street;
                this.activityData['house'] = this.house;
            }
            
            if(this.edit_activity){
                this.activityData['id'] = this.edit_activity.org_activity.id
            }

            this.activityData['isMain'] = this.isMain;
           
            axios.post( "/profile/changeOrganizationActivity", this.activityData)
            .then( (response) => {
                if(response.data['response'] == 'success'){
                    this.back();
                }
            });  
        },
        formIsValid(){
            let isValid = true;
            for(let propertyIsValid of this.select_validation_sripts){
                if(!this[propertyIsValid]){
                    isValid = false;
                }
            }
            return isValid;
        }
    },
    watch: {
        selectActivity(){ 
            this.select_script =  this.activities_scripts[this.selectActivity];
            this.select_validation_sripts =  this.validation_sripts[this.selectActivity];
            this.showErrors = false;
            this.activityData = {};
            this.activityData['select_activity'] = this.selectActivity;

            axios.post( "/api/get_materials", {
                params: {
                    activity: this.selectActivity
                }
            }).then( (response) => {
                if(response.data){
                    this.materials = response.data;
                    return;
                }
            });      
        },
        edit_activity(){
            
            if(this.edit_activity){
                this.setMaterials(this.edit_activity.materials || []);
                this.setVehicleTypes(this.edit_activity.vehiclesTypes || []);
                this.setVehicles(this.edit_activity.vehicles || []);
                this.setCopacities(this.edit_activity.copacities || []);
                this.set_bilding_branches(this.edit_activity.bildingBranches || []);
               
            }
        }
    },
    computed: {
        bildingBranchesValid(){
            return !!this.select_bilding_branches.length;
        },
        bildingBranchesShowError(){
            return !this.bildingBranchesValid && this.showErrors;
        },

        copacitiesValid(){
            return !!this.selectCopacities.length;
        },
        copacitiesShowError(){
             return !this.copacitiesValid && this.showErrors;
        },

        vehiclesTypeValid(){
            return !!this.selectedVehiclesType.length;
        },
        vehiclesTypeShowError(){
             return !this.vehiclesTypeValid && this.showErrors;
        },

        materialsValid(){
            return !!this.selectedMaterials.length;
        },
        materialsShowError(){
            return !this.materialsValid && this.showErrors;
        },

        cityValid(){
            return !!this.city;
        },
        cityError(){
            if(!this.cityValid && this.showErrors){
                return 'is-error';
            }
            return '';
        },

        streetValid(){
            return !!this.street;
        },
        streetError(){
            if(!this.streetValid && this.showErrors){
                return 'is-error';
            }
            return '';
        },

        houseValid(){
            return !!this.house;
        },
        houseError(){
            if(!this.houseValid  && this.showErrors){
                return 'is-error';
            }
            return '';
        }
    },
    mounted() {
        console.log(this.edit_activity_id);
        breadCrumbs.make('profile');

        eventHub.$on('BCBackToProfile', (params) => {
            this.$emit('showProfile');      
        })    
    },
    created() {
       
        axios.post('/api/get_activities', {

        }).then((response) => {
            this.activities = response.data;
            if(!this.edit_activity){
                this.selectActivity = this.activities[Object.keys(this.activities)[0]].slug;
                return;
            }
            this.selectActivity = this.edit_activity.activity.slug;
        }); 

        axios.get( "/api/get_vehicle_types", {
        
        }).then( (response) => {
            if(response.data){
                this.vehicle_types = response.data;
                return;
            }
        });

        // axios.get( "/api/get_vehicles", {
        
        // }).then( (response) => {
        //     this.vehicles = response.data;
        // });

        // axios.get( "/api/get_copacities", { 
        // }).then( (response) => {
        //     if(response.data){
        //         this.copacities = response.data;
        //         return;
        //     }
        // }); 

        axios.get( "/api/get_bilding_branch", { 
        }).then( (response) => {
            if(response.data){
                this.bilding_branches = response.data;
                return;
            }        
        });

        if(this.edit_activity){
            console.log(this.edit_activity)
            this.setMaterials(this.edit_activity.materials.map((item)=> item.id) || []);
            this.setVehicleTypes(this.edit_activity.vehiclesTypes.map((item)=> item.id) || []);
            this.setVehicles(this.edit_activity.vehicles.map((item)=> item.id) || []);
            this.setCopacities(this.edit_activity.copacities.map((item)=> item.id) || []);
            this.set_bilding_branches(this.edit_activity.bildingBranches.map((item)=> item.id) || []);
            this.city = this.edit_activity.org_activity.city || '';
            this.street = this.edit_activity.org_activity.street || '';
            this.house = this.edit_activity.org_activity.house || '';
          
                console.log(this.edit_activity.org_activity.isMain);
                this.isMain = !!this.edit_activity.org_activity.isMain;
        }
        document.body.scrollIntoView({behavior: 'smooth'});
       
    },
    
}
</script>