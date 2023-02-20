<template>
  <div class="flex flex-col h-screen max-h-screen">
    <!-- Search / Results -->
    <div class="z-20 flex justify-center relative bg-hero-pattern bg-cover px-4 pt-8 pb-32">
      <!-- Search Input -->
      <div class="w-full max-w-screen-sm">
        <h1 class="text-white text-center text-5xl pb-4 font-bold tracking-[3px]">IP Address Tracker</h1>
        <div class="flex">
          <input v-model="queryIp" type="text" class="flex-1 py-3 px-2 rounded-tl-md rounded-bl-md focus:outline-none"
            placeholder="Search for any Ip address or leave empty to get your ip info">
          <i @click="getIpInfo" class="cursor-pointer bg-black text-white px-4 
                    rounded-tr-md rounded-br-md flex items-center fas fa-chevron-right"></i>
        </div>
      </div>
      <!--IP Info-->
      <IpInfo v-if="ipInfo" v-bind:ipInfo="ipInfo" />
    </div>

    <!-- Map -->
    <div id="mapid" class="h-full z-10"></div>
  </div>
</template>

<script>
import IpInfo from '@/components/IpInfo.vue';
import leaflet from 'leaflet';
import { onMounted, ref } from 'vue';
import axios from 'axios';

export default {
  name: 'Home',
  components: {
    IpInfo
  },
  setup() {
    let mymap;

    const queryIp = ref("")
    const ipInfo = ref(null)
    onMounted(() => {
      mymap = leaflet.map('mapid').setView([51.505, -0.09], 13);

      leaflet
        .tileLayer(
          "", //Mapbox API goes here
          {
            attribution:
              'Map data &copy; <a href="https://www.openstreetmap.org/copyright">OpenStreetMap</a> contributors, Imagery © <a href="https://www.mapbox.com/">Mapbox</a>',
            maxZoom: 18,
            id: "mapbox/streets-v11",
            tileSize: 512,
            zoomOffset: -1,
            accessToken:
              "",// MAPBOAPIACCESSTOKEN
          }
        )
        .addTo(mymap);
    });

  // gets ip information from API
  const getIpInfo = async () => {
      try {
        const data = await axios.get(
          `=${queryIp.value}` //IP API goes here
        );
        const result = data.data;
        ipInfo.value = {
          address: result.ip,
          state: result.location.region,
          timezone: result.location.timezone,
          isp: result.isp,
          lat: result.location.lat,
          lng: result.location.lng,
        };
        leaflet.marker([ipInfo.value.lat, ipInfo.value.lng]).addTo(mymap); // adds marker to map
        mymap.setView([ipInfo.value.lat, ipInfo.value.lng], 13); // sets map view to marker
      } catch (err) {
        alert(err.message); // shows error message
      }
    };
    return { queryIp, ipInfo, getIpInfo };  
  }
};
</script>