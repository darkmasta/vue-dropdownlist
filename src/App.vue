<template>
    <div>
      <div id='dropdownlist' style="margin:50px auto 0; width:250px;">
        <br>
        <ejs-dropdownlist popupHeight="200px" popupWidth="250px" 
          :dataSource='CarDropdownDataYear' :fields="fieldsYear"  placeholder='Years'
          sortOrder='Descending' :select="onYearSelect">
        </ejs-dropdownlist>
        <ejs-dropdownlist popupHeight="200px" popupWidth="250px" 
          :dataSource='CarDropdownDataMake' :fields="fieldsMake"  placeholder='Make'
        sortOrder='Descending' :select="onMakeSelect">
        </ejs-dropdownlist>
        <ejs-dropdownlist popupHeight="200px" popupWidth="250px" 
          :dataSource='CarDropdownDataModel' :fields="fieldsModel"  placeholder='Model'
          sortOrder='Descending'>
        </ejs-dropdownlist>
      </div>
    </div>
</template>

<script>
import Vue from 'vue';
import { DropDownListPlugin } from "@syncfusion/ej2-vue-dropdowns";
Vue.use(DropDownListPlugin);
import axios from 'axios'
import VueAxios from 'vue-axios'
 
Vue.use(VueAxios, axios)

export default Vue.extend({
  data: function() {
    return {
      CarDropdownDataYear: [],
      CarDropdownDataModel: [],
      CarDropdownDataMake: [],
      selectedYear: null,
      selectedMake: null,
      fieldsYear: { text: 'year', value: 'id' },
      fieldsMake: { text: 'make', value: 'id' },
      fieldsModel: { text: 'model', value: 'id' }
    };
  },
  mounted() {
    
    axios.get('https://cors-anywhere.herokuapp.com/https://rateengine.ship.cars/v2/vehicles/years/?token=5cbe12fb62f4941267d623499a2a4fd5948fd3ef')
          .then(response => { 
            var data = response.data
            for (var i = 0; i < data.length; i++) {
              let tmp = {"year": 0, id: 0}
              tmp.year = data[i].year
              tmp.id = i 
              this.CarDropdownDataYear.push(tmp)
            }
          })

  },
  computed: {
  },
  methods: {
    onYearSelect: function(event) {
      this.selectedYear = event.itemData.year

      var url = 'https://cors-anywhere.herokuapp.com/https://rateengine.ship.cars/v2/vehicles/makes/?year=' + event.itemData.year + '&token=5cbe12fb62f4941267d623499a2a4fd5948fd3ef'
      axios.get(url)
        .then(response => { 
          var data = response.data
          for (var i = 0; i < data.length; i++) {
            let tmp = {"make": "", id: 0}
            tmp.make = data[i].make
            tmp.id = i 
            this.CarDropdownDataMake.push(tmp)
          }
        })
    },
    onMakeSelect: function(event) {
      this.selectedMake = event.itemData.make
      var url = 'https://cors-anywhere.herokuapp.com/https://rateengine.ship.cars/v2/vehicles/models/?year=' + this.selectedYear + '&make=' + this.selectedMake + '&token=5cbe12fb62f4941267d623499a2a4fd5948fd3ef'
      axios.get(url)
        .then(response => { 
          var data = response.data
          for (var i = 0; i < data.length; i++) {
            let tmp = {"model": "", id: 0}
            tmp.model = data[i].model
            tmp.id = i 
            this.CarDropdownDataModel.push(tmp)
          }
        })
    }
  }
});

</script>
<style>
@import url(https://cdn.syncfusion.com/ej2/material.css);
</style>