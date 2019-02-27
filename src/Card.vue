<template>
  <div>
      <b-container v-if="currentListing" class="single-page">
          <b-row>
              <b-col>
                  <h1>
                      {{currentListing.fields['Title/Topic']}}
                  </h1>
              </b-col>
          </b-row>
          <b-row>
              <b-col sm="8">
                   <b-img :src="imageURL" fluid alt="Responsive image" />
              </b-col>
          </b-row>
          <b-row>
              <b-col sm="8">
                  <h1>Description</h1>
                  <p>{{currentListing.fields.Clipping}}</p>
                  <ul>
                      <li>Date Added: {{currentListing.fields['Date Added']}}</li>
                      <li>Lense: {{currentListing.fields['Lens-str']}}</li>
                      <li>Link: {{currentListing.fields['Link 1']}}</li>
                  </ul>
              </b-col>
          </b-row>
          <b-row>
            <b-col sm="8">
            </b-col>
          </b-row>

      </b-container>
  </div>
</template>

<script>
import axios from 'axios';
export default {
  name: "Card",
  data() {
    return {
        id: null,
        listings: []
    };
  },
  computed: {
      currentListing() {
        return this.listings[this.listings.findIndex(x => x.id === this.id)];
      },
      imageURL() {
        if(this.currentListing)
            if(this.currentListing.fields.Attachment)
                return this.currentListing.fields.Attachment[0].thumbnails.large.url
        return 'https://picsum.photos/600/300/?image=25'

      }
  },
  mounted() {
      this.id = this.$route.params.id;
      this.getListings();
  },
  methods: {
      getListings(){
          var self = this;
            var app_id = "appH81X67TStprrkF";
            var app_key = "key0Uo9OP77Cxoi5c";
            axios.get(
                "https://api.airtable.com/v0/"+app_id+"/Weekly%20Report?maxRecords=100&view=Main%20View",
                {
                    headers: { Authorization: "Bearer "+app_key }
                }
            ).then(function(response){
                console.log(response.data.records);
                self.listings = response.data.records;
            }).catch(function(error){
                console.log(error)
            });

      },
  }
};
</script>
