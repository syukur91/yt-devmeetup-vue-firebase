<template>
  <v-container>
    <v-layout row wrap>
      <v-flex xs12>
        <v-card>
          <v-card-title>
            <h6 class="primary--text">{{ meetup.campaignName }}</h6>
          </v-card-title>
          <v-card-media
            :src="meetup.imageUrl"
            height="400px"
          ></v-card-media>
          <v-card-text>
            <!-- <div class="info--text">{{ meetup.date | date }} - {{ meetup.campaignType }}</div> -->
            <div class="black--text">Balance: {{ meetup.quantity }}</div>
            <div class="black--text">Already Showed: 5</div>
            <div class="black--text">Radius: {{ meetup.radius }}</div>
            <div class="black--text">Latitude: {{ meetup.latitude }} </div>
            <div class="black--text">Longitude: {{ meetup.longitude }} </div>
            <!-- <div>{{ meetup.description }}</div> -->
          </v-card-text>
        </v-card>
      </v-flex>
    </v-layout>
  </v-container>
</template>

<script>
  export default {
    props: ['id'],
    computed: {
      meetup () {
        var item = this.$store.getters.loadedMeetup(this.id)
        return item
      }
    }
  }
</script>
