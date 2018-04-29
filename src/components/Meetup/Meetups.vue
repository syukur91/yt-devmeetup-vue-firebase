<template>
  <v-container>
    <v-layout row wrap v-for="meetup in meetups" :key="meetup.id" class="mb-2">
      <v-flex xs12 sm10 md8 offset-sm1 offset-md2>
        <v-card class="item">
          
            <v-layout row>
           <v-flex xs12>
            <v-card color="cyan darken-2" class="white--text">
              <v-container fluid grid-list-lg>
                <v-layout row>
                  <v-flex xs5>
                    <v-card-media
                  :src="meetup.imageUrl"
                  height="120px"
                ></v-card-media>
                  </v-flex>
                  <v-flex xs7>
                    <div>
                      <div>
                        <h3 class="grey--text headline">{{ meetup.campaignName }}</h3>
                        <!-- <h3 class="grey--text mb-0">{{ meetup.campaignName }}</h3> -->
                        <!-- <div>Created: {{ meetup.date | date }}</div> -->
                        <!-- <div class="black--text">Balance: {{ meetup.quantity }}</div>
                        <div class="black--text">Already Showed: 5</div>
                        <div class="black--text">Radius: {{ meetup.radius }}</div>
                        <div class="black--text">Latitude: {{ meetup.latitude }} </div>
                        <div class="black--text">Longitude: {{ meetup.longitude }} </div> -->
                      </div>
                    </div>
                      <v-card-actions>
                        <v-btn flat :to="'/meetups/' + meetup.id">
                          <!-- <v-icon left light>arrow_forward</v-icon> -->
                            <v-icon left light>info</v-icon>
                          View Ads Detail
                        </v-btn>
                      </v-card-actions>
                  </v-flex>
                </v-layout>
              </v-container>
            </v-card>
          </v-flex>

            </v-layout>
          
        </v-card>
      </v-flex>
    </v-layout>
  </v-container>
</template>

<script>
  export default {
    computed: {
      meetups () {
        return this.$store.getters.loadedMeetups
      }
    }
  }
</script>
