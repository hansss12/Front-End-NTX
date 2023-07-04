<template>
  <div class="h-screen relative">
    <GeoErrorModal
      v-if="geoError"
      :geoErrorMsg="geoErrorMsg"
      @closeGeoError="closeGeoError"
    />
    <MapFeatures
      :fetchCoords="fetchCoords"
      :coords="coords"
      @getGeolocation="getGeolocation"
      :fetchAttackCoords="fetchAttackCoords"
      :attackCoords="attackCoords"
      @getAttacklocation="getAttacklocation"
      class="w-full md:w-auto absolute md:top-[40px] md:left-[60px] z-[2]"
    />
    <div id="mapid" class="h-full z-[1]"></div>
  </div>
</template>

<script>
import leaflet from "leaflet";
import { onMounted, ref } from "vue";
import axios from "axios"
import GeoErrorModal from "../components/GeoErrorModal.vue";
import MapFeatures from "../components/MapFeatures.vue";
export default {
  name: "HomeView",
  components: { GeoErrorModal, MapFeatures },
  setup() {
    let map;
    onMounted(() => {
      map = leaflet
        .map("mapid", {
          zoomControl: false,
        })
        .setView([28.538336, -81.379234], 10);

      leaflet
        .tileLayer(
          `https://api.mapbox.com/styles/v1/{id}/tiles/{z}/{x}/{y}?access_token=${process.env.VUE_APP_API_KEY}`,
          {
            maxZoom: 18,
            id: "mapbox/streets-v11",
            tileSize: 512,
            zoomOffset: -1,
            accessToken: process.env.VUE_APP_API_KEY,
          }
        )
        .addTo(map);

      getGeolocation();
    });

    // set up ref value
    const coords = ref(null);
    const fetchCoords = ref(null);
    const geoMarker = ref(null);
    const attackMarker = ref(null);
    const geoError = ref(null);
    const geoErrorMsg = ref(null);
    const fetchAttackCoords = ref(null);
    const attackCoords = ref(null);

    const getGeolocation = () => {
      if (!coords.value) {
        // session storage checking
        if (sessionStorage.getItem("coords")) {
          coords.value = JSON.parse(sessionStorage.getItem("coords"));
          plotGeoLocation(coords.value);
          return;
        }

        // else get coords from geolocation API
        fetchCoords.value = true;
        navigator.geolocation.getCurrentPosition(setCoords, getLocError);
        return;
      }

      // remove location
      coords.value = null;
      sessionStorage.removeItem("coords");
      map.removeLayer(geoMarker.value);
    };

    const getAttacklocation = () => {
      if (!attackCoords.value) {
        // get attackCoords from back end api
        fetchAttackCoords.value = true;
        setAttackCoords()
        return;
      }

      // remove location
      attackCoords.value = null;
      map.removeLayer(attackMarker.value);
    };

    const setCoords = (pos) => {
      fetchAttackCoords.value = null;

      // set coords in session storage
      const setSessionCoords = {
        lat: pos.coords.latitude,
        lng: pos.coords.longitude,
      };
      sessionStorage.setItem("coords", JSON.stringify(setSessionCoords));

      // set ref coords value
      coords.value = setSessionCoords;

      plotGeoLocation(coords.value);
    };

    const setAttackCoords = async () => {
      fetchAttackCoords.value = null;

      // fetch coords with axios
      const { data } = await axios.get("http://localhost:3000/")

      // set ref attackCoords value
      attackCoords.value = data;
      plotAttackGeoLocation(attackCoords.value);
    };

    const getLocError = (error) => {
      fetchCoords.value = null;
      geoError.value = true;
      geoErrorMsg.value = error.message;
    };

    const plotGeoLocation = (coords) => {
      // create custom marker
      const customMarker = leaflet.icon({
        iconUrl: require("../assets/map-marker-blue.svg"),
        iconSize: [35, 35],
      });
      geoMarker.value = leaflet
        .marker([coords.lat, coords.lng], { icon: customMarker })
        .addTo(map);
      map.setView([coords.lat, coords.lng], 10);
    };

    const plotAttackGeoLocation = (coords) => {
      // create custom marker
      const customMarker = leaflet.icon({
        iconUrl: require("../assets/map-marker-red.svg"),
        iconSize: [30, 30],
      });

      // looping the cords data
      for (let index = 0; index < coords.length; index++) {
        const element = coords[index];        
        attackMarker.value = leaflet
          .marker([element.latitude, element.longitude], { icon: customMarker })
          .addTo(map);
        map.setView([element.latitude, element.longitude], 10);
      }
    };


    const closeGeoError = () => {
      geoErrorMsg.value = null;
      geoError.value = null;
    };

    return {
      geoError,
      closeGeoError,
      geoErrorMsg,
      fetchCoords,
      coords,
      getGeolocation,
      fetchAttackCoords,
      attackCoords,
      getAttacklocation
    };
  },
};
</script>
