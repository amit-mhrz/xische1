<template>

  <div class="filters">      
      <b-row>

          <b-col cols="6" md="3">
            <h2>CONTENT TYPE</h2>
              <b-form-group>
                  <b-row>
                      <b-col v-for="(topic, i) in availableTopics" :key="i">
                          <input type="checkbox" :value="topic.value" v-model="selectedTopics">{{topic.text}}
                      </b-col>
                  </b-row>
              </b-form-group>
          </b-col>

          <b-col cols="6" md="3">
            <h2>LENSES</h2>
              <b-form-group>
                  <b-row>
                      <b-col v-for="(lens, i) in newLens" :key="i">
                          <input type="checkbox" :value="lens.value" v-model="selectedLens1"> {{lens.text}}
                      </b-col>
                  </b-row>
              </b-form-group>
          </b-col>

          <b-col cols="6" md="3">
            <h2>CITY</h2>
            <b-form-group>
              <b-form-checkbox-group plain stacked v-model="selected" :options="options" />
            </b-form-group>
          </b-col>

          <b-col cols="6" md="3">
            <h2>FOCUS AREA</h2>
          </b-col>

      </b-row>

          <b-row>
            <b-col sm="12">
               <b-form-input
                   type="text"
                      v-model="searchText"
                      placeholder="Search">
                  </b-form-input>
              </b-col>
          </b-row>
          <br>
          <b-row>
              <b-col sm="6">
                <b-form-select
                      :options="availableLens"
                      required
                      v-model="selectedLens">
                </b-form-select>
              </b-col>
              <b-col sm="6">
                <b-form-select
                      :options="availablePublications"
                      required
                      v-model="selectedPublication">
                </b-form-select>
              </b-col>
          </b-row>
          <br>
                    
      <br><br>
  </div>
</template>

<script>
import axios from 'axios';
export default {
  name: "filters",
  data() {
    return {
      searchText: "",
      availableTopics: [
      ],
      newLens: [
      ],
      availableLens: [
        { value: null, text: 'Please select a lense' },
      ],
      availablePublications: [
        { value: null, text: 'Please select a publication' },
      ],
      selectedTopics: [],
      selectedLens1: [],
      dateFilter: "decending",
      selectedLens: null,
      selectedPublication: null,

      selected: [], // Must be an array reference!
        options: [
          { text: 'London', value: 'london' },
          { text: 'Tokyo', value: 'tokyo' },
          { text: 'Dubai', value: 'dubai' },
          { text: 'Rome', value: 'rome' }
        ]

    };

  },
  mounted() {
    this.getLenses();
    this.getPublications();
    this.getTypes();
    this.getLens();
  },
  methods: {
    getLenses(){
      var self = this;
      var app_id = "appH81X67TStprrkF";
      var app_key = "key0Uo9OP77Cxoi5c";
      axios.get(
          "https://api.airtable.com/v0/"+app_id+"/Lenses",
          {
              headers: { Authorization: "Bearer "+app_key }
          }
      ).then(function(response){
        response.data.records.forEach(element => {
          self.availableLens.push({text: element.fields.Name, value: element.fields.Name})
        });
      }).catch(function(error){
        console.log(error)
      });
    },
    getPublications(){
      var self = this;
      var app_id = "appH81X67TStprrkF";
      var app_key = "key0Uo9OP77Cxoi5c";
      axios.get(
          "https://api.airtable.com/v0/"+app_id+"/Sources",
          {
              headers: { Authorization: "Bearer "+app_key }
          }
      ).then(function(response){
        console.log(response.data.records);
        response.data.records.forEach(element => {
          self.availablePublications.push({text: element.fields.Name, value: element.id})
        });
      }).catch(function(error){
        console.log(error)
      });
    },
    getTypes(){
      var self = this;
      var app_id = "appH81X67TStprrkF";
      var app_key = "key0Uo9OP77Cxoi5c";
      axios.get(
          "https://api.airtable.com/v0/"+app_id+"/Type",
          {
              headers: { Authorization: "Bearer "+app_key }
          }
      ).then(function(response){
        console.log(response.data.records);
        response.data.records.forEach(element => {
          self.availableTopics.push({text: element.fields.Name, value: element.fields.Name})
        });
      }).catch(function(error){
        console.log(error)
      });
    },
    getLens(){
      var self = this;
      var app_id = "appH81X67TStprrkF";
      var app_key = "key0Uo9OP77Cxoi5c";
      axios.get(
          "https://api.airtable.com/v0/"+app_id+"/Lenses",
          {
              headers: { Authorization: "Bearer "+app_key }
          }
      ).then(function(response){
        response.data.records.forEach(element => {
          self.newLens.push({text: element.fields.Name, value: element.fields.Name})
        });
      }).catch(function(error){
        console.log(error)
      });
    }
  },
  watch: {
    searchText() {
      this.$emit("searched", this.searchText);
    },
    selectedTopics() {
      this.$emit("topicSelected", this.selectedTopics);
    },
    selectedLens1() {
      this.$emit("lensSelected1", this.selectedLens1);
    },
    dateFilter() {
      this.$emit("dateFilter", this.dateFilter);
    },
    selectedLens() {
      this.$emit("lensSelected", this.selectedLens);
    },
    selectedPublication() {
      this.$emit("publicationSelected", this.selectedPublication);
    },
  }
};
</script>