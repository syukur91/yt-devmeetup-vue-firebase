<template>
  <v-container>
    <v-layout row>
      <v-flex xs12 sm6 offset-sm3>
        <h4>Create a new Campaign</h4>
      </v-flex>
    </v-layout>
    <v-layout row>
      <v-flex xs12>
        <form @submit.prevent="onCreateMeetup">
          <v-layout row>
            <v-flex xs12 sm6 offset-sm3>
              <v-text-field
                name="campaignName"
                label="Campaign Name"
                id="campaignName"
                v-model="campaignName"
                required></v-text-field>
            </v-flex>
          </v-layout>
          <v-layout row>
            <v-flex xs12 sm6 offset-sm3>
              <v-text-field
                name="campaignType"
                label="Campaign Type"
                id="campaignType"
                v-model="campaignType"
                required></v-text-field>
            </v-flex>
          </v-layout>
          <v-layout row>
            <v-flex xs12 sm6 offset-sm3>
              <v-text-field
                name="quantity"
                label="Quantity"
                id="quantity"
                v-model="quantity"
                type="number"
                required></v-text-field>
            </v-flex>
          </v-layout>
            <v-layout row>
            <v-flex xs12 sm6 offset-sm3>
              <v-text-field
                name="radius"
                label="Radius"
                id="radius"
                v-model="radius"
                type="number"
                required></v-text-field>
            </v-flex>
          </v-layout>
          <v-layout row>
            <v-flex xs12 sm6 offset-sm3>
            
              <v-btn raised class="primary" @click="onPickFile">Upload Image</v-btn>
              <input
                type="file"
                style="display: none"
                ref="fileInput"
                accept="image/*"
                @change="onFilePicked">

            </v-flex>
          </v-layout>
          <v-layout row>
            <v-flex xs12 sm6 offset-sm3>
              <img :src="imageUrl" height="150">
            </v-flex>
          </v-layout>

          <v-layout row>

          <v-flex xs12 sm6 offset-sm3>
          <span>Location: </span>
           <gmap-map
            :center="{lat:10, lng:10}"
            :zoom="7"
            @rightclick="mapRclicked"
            map-type-id="terrain"
            style="width: 500px; height: 300px">
            <gmap-marker 
            :key="index"
            v-if="m.enabled"
            :position.sync="m.position"
            :opacity="m.opacity"
            :draggable.sync="m.draggable"
            @click="m.clicked++"
            @rightclick="m.rightClicked++"
            @dragend="m.dragended++"
            v-for="(m, index) in markers"
            >
            </gmap-marker>
          </gmap-map>
              
          </v-flex>
          
         
          
          </v-layout>
         
         <!-- <v-layout row>
            <v-flex xs12 sm6 offset-sm3>
              <h4>Choose a Data & Time</h4>
            </v-flex>
          </v-layout>
          <v-layout row class="mb-2">
            <v-flex xs12 sm6 offset-sm3>
              <v-date-picker v-model="date"></v-date-picker>
            </v-flex>
          </v-layout>
          <v-layout row>
            <v-flex xs12 sm6 offset-sm3>
              <v-time-picker v-model="time" format="24hr"></v-time-picker>
            </v-flex>
          </v-layout>-->
          <v-layout row> 
            <v-flex xs12 sm6 offset-sm3>
              <v-btn
                class="primary"
                :disabled="!formIsValid"
                type="submit">Create Meetup</v-btn>
            </v-flex>
          </v-layout>
        </form>
      </v-flex>
    </v-layout>
  </v-container>
</template>

<script>
  export default {
    data () {
      return {
        campaignName: '',
        campaignType: '',
        imageUrl: '',
        quantity: 0,
        // date: new Date(),
        // time: new Date(),
        image: null,
        radius: 0,
        markers: []
      }
    },
    filters: {
      markerRemover (markers) {
        if (this.markersEven) {
          const result = []
          for (var i = 0; i < markers.length; i += 2) {
            result.push(markers[i])
          }
          return result
        } else {
          return markers
        }
      }
    },
    computed: {
      formIsValid () {
        return this.campaignName !== '' &&
          this.campaignType !== '' &&
          this.imageUrl !== '' &&
          this.quantity !== '' &&
          this.radius !== ''
      },
      submittableDateTime () {
        const date = new Date(this.date)
        if (typeof this.time === 'string') {
          let hours = this.time.match(/^(\d+)/)[1]
          const minutes = this.time.match(/:(\d+)/)[1]
          date.setHours(hours)
          date.setMinutes(minutes)
        } else {
          date.setHours(this.time.getHours())
          date.setMinutes(this.time.getMinutes())
        }
        return date
      }
    },
    methods: {
      onCreateMeetup () {
        if (!this.formIsValid) {
          return
        }
        if (!this.image) {
          return
        }
        const meetupData = {
          campaignName: this.campaignName,
          campaignType: this.campaignType,
          image: this.image,
          quantity: this.quantity,
          // date: this.submittableDateTime,
          radius: this.radius
        }
        this.$store.dispatch('createMeetup', meetupData)
        this.$router.push('/meetups')
      },
      onPickFile () {
        this.$refs.fileInput.click()
      },
      onFilePicked (event) {
        const files = event.target.files
        let filename = files[0].name
        if (filename.lastIndexOf('.') <= 0) {
          return alert('Please add a valid file!')
        }
        const fileReader = new FileReader()
        fileReader.addEventListener('load', () => {
          this.imageUrl = fileReader.result
        })
        fileReader.readAsDataURL(files[0])
        this.image = files[0]
      },
      mapRclicked (mouseArgs) {
        const createdMarker = this.addMarker()
        createdMarker.position.lat = mouseArgs.latLng.lat()
        createdMarker.position.lng = mouseArgs.latLng.lng()
        console.log(createdMarker.position.lat)
      },
      addMarker: function addMarker () {
        this.markers.push({
          position: { lat: 48.8538302, lng: 2.2982161 },
          opacity: 1,
          draggable: true,
          enabled: true,
          clicked: 0,
          rightClicked: 0,
          dragended: 0,
          ifw: true,
          ifw2text: 'This text is bad please change me :( '
        })
        return this.markers[this.markers.length - 1]
      }
    }
  }
</script>
