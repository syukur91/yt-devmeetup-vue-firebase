<template>
  <div>
    <v-data-table
      :headers="headers"
      :items="desserts"
      :search="search"
      :pagination.sync="pagination"
      class="elevation-1"
    >
      <template slot="items" slot-scope="props">
        <td class="text-xs-right">{{ props.item.campaignName }}</td>
        <td class="text-xs-right">{{ props.item.radius }}</td>
        <td class="text-xs-right">{{ props.item.latitude }}</td>
        <td class="text-xs-right">{{ props.item.longitude }}</td>
        <td class="justify-center layout px-0">
          <v-icon small class="mr-2" @click="editItem(props)">
            info
          </v-icon>
        </td>
      </template>
    </v-data-table>
    <!-- <div class="text-xs-center pt-2">
      <v-btn color="primary" @click.native="toggleOrder">Toggle sort order</v-btn>
      <v-btn color="primary" @click.native="nextSort">Sort next column</v-btn>
    </div> -->
  </div>
</template>

<script>
  export default {
    data () {
      return {
        search: '',
        pagination: {
          sortBy: 'radius'
        },
        selected: [],
        headers: [
          { text: 'Name', value: 'campaignName' },
          { text: 'Radius', value: 'radius' },
          { text: 'Latitude', value: 'latitude' },
          { text: 'Longitude', value: 'longitude' },
          { text: 'Action', value: 'action' }
        ],
        desserts: this.$store.getters.loadedMeetups
      }
    },
    methods: {
      toggleOrder () {
        this.pagination.descending = !this.pagination.descending
      },
      nextSort () {
        let index = this.headers.findIndex(h => h.value === this.pagination.sortBy)
        index = (index + 1) % this.headers.length
        index = index === 0 ? index + 1 : index
        this.pagination.sortBy = this.headers[index].value
      },
     editItem (item) {
        this.$router.push('/meetups/'+item.item.id)
      }

    }
  }
</script>
