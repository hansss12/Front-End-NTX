<template>
  <!-- Navbar -->
  <div class="grid grid-cols-12">
    <div class="col-span-1 border-r border-b border-neutral-700 flex justify-center">
      <h1 class="text-neutral-300 p-5 text-xl">NTX</h1>
    </div>
    <div class="flex col-span-10 border-b border-neutral-700">
      <div class="flex justify-start pl-10">
        <i class="fab fa-facebook text-white text-xl py-5 px-2"></i>
        <i class="fab fa-github text-white text-xl py-5 px-2"></i>
        <i class="fab fa-instagram text-white text-xl py-5 px-2"></i>
      </div>
      <div class="flex justify-center w-full">
        <img src="https://www.ntxsolution.com/playground_assets/Logo-NTX-panjang.png" alt="logo" class="w-[16vw] h-[8vh]">
        <h1 class="text-neutral-300 p-5 text-xl"></h1>
      </div>
    </div>
    <div class=" col-span-1 border-b border-neutral-700">
      <div class="flex justify-end">
        <i class="fas fa-bars text-white text-xl p-5 bg-[#f57b3a]"></i>
      </div>
    </div>
  </div>
  <!-- Navbar end -->

  <div class="w-screen mt-[15vh] px-20">
    <!-- hero -->
    <div class="flex">
      <h1 class="font-semibold text-9xl text-white">NTX Solutions</h1>
      <h1 class="text-white max-w-[27vw] text-sm text-start font-thin px-5 mt-10">
        Using advanced technologies to automate and optimize business processes through data analysis
        and intelligent decision-making to improve efficiency, reduce costs, and enhance customer experience.
      </h1>
    </div>
    <div class="flex">
      <div class="flex flex-col justify-center">
        <h1 class="bg-[#f57b3a] rounded-full w-fit h-fit py-3 px-8 font-semibold text-2xl">For Your</h1>
      </div>
      <h1 class="font-semibold text-9xl text-white pl-5">Technology <span class="text-[#f57b3a]">& AI</span></h1>
      <div class="flex flex-col justify-center">
        <i class="far fa-chart-bar text-white text-center border rounded-3xl border-[#f57b3a] p-8 ml-5 text-4xl"></i>
      </div>
    </div>
    <!-- hero end -->
    <!-- map data analytics -->
    <div class="flex justify-center mt-[10vh] mb-10">
      <h1 class="text-white text-4xl font-medium">Data analytics example on <span
          class="text-[#f57b3a]">cyberattack</span> in
        2023</h1>
    </div>
    <div class="grid grid-cols-4 mb-10">
      <div class="h-[60vh] relative w-full col-span-4">
        <GeoErrorModal v-if="geoError" :geoErrorMsg="geoErrorMsg" @closeGeoError="closeGeoError" />
        <MapFeatures :fetchCoords="fetchCoords" :coords="coords" @getGeolocation="getGeolocation"
          :fetchAttackCoords="fetchAttackCoords" :attackCoords="attackCoords" @getAttacklocation="getAttacklocation"
          class="w-full md:w-auto absolute md:top-[40px] md:left-[60px] z-[2]" />
        <div id="mapid" class="h-full z-[1]"></div>
      </div>
    </div>
    <div class="flex justify-center mb-36">
      <h1 class="text-white text-2xl font-medium">click this icon <i
          class="fas fa-map-marker-alt text-[#f57b3a] text-2xl px-1"></i> to show <span
          class="text-[#f57b3a]">cyberattack</span> location</h1>
    </div>
    <!-- map data end -->

    <!-- services -->
    <div class="flex justify-center mb-10">
      <h1 class="text-8xl text-white font-semibold">Services</h1>
    </div>

    <div class="flex justify-center">
      <h1 class="text-white text-4xl text-center">
        Our team of experts has extensive experience in <span class="text-[#f57b3a]">developing and implementing AI
          technologies,</span> and we work closely
        with our clients to deliver customized <span class="text-[#f57b3a]">solutions that meet their specific
          needs.</span> We can help you with:
      </h1>
    </div>

    <div class="grid grid-cols-3 mt-20 mb-20">
      <div class="p-5" v-for="a, i in 3" :key="a">
        <img :src="data[i].img" alt="images">
        <h1 class="text-white text-4xl font-semibold my-5">{{ data[i].title }}</h1>
        <h1 class="text-white text-base font-light">
          {{ data[i].desc }}
        </h1>
      </div>
    </div>
    <!-- services end -->
  </div>

  <!-- footer -->
  <footer class="rounded-lg shadow m-4">
    <div class="w-full max-w-screen-xl mx-auto p-4 md:py-8">
      <div class="sm:flex sm:items-center sm:justify-between">
        <a href="https://www.ntxsolution.com/" class="flex items-center mb-4 sm:mb-0">
          <img src="https://www.ntxsolution.com/playground_assets/Logo-NTX-panjang.png" class="h-16 mr-3"
            alt="Flowbite Logo" />
        </a>
        <ul class="flex flex-wrap items-center mb-6 text-sm font-medium text-gray-500 sm:mb-0 dark:text-gray-400">
          <li>
            <a href="#" class="mr-4 hover:underline md:mr-6 ">About</a>
          </li>
          <li>
            <a href="#" class="mr-4 hover:underline md:mr-6">Privacy Policy</a>
          </li>
          <li>
            <a href="#" class="mr-4 hover:underline md:mr-6 ">Licensing</a>
          </li>
          <li>
            <a href="#" class="hover:underline">Contact</a>
          </li>
        </ul>
      </div>
      <hr class="my-6 border-gray-200 sm:mx-auto dark:border-gray-700 lg:my-8" />
      <span class="block text-sm text-gray-500 sm:text-center dark:text-gray-400">© 2023 <a
          href="https://www.ntxsolution.com/" class="hover:underline">NTX Solusi Teknologi™</a>. All Rights
        Reserved.</span>
    </div>
  </footer>
  <!-- footer end -->
</template>

<script>
import leaflet from "leaflet";
import { onMounted, ref } from "vue";
import axios from "axios"
import GeoErrorModal from "../components/GeoErrorModal.vue";
import MapFeatures from "../components/MapFeatures.vue";
import data from "./data";
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
      getAttacklocation,
      data
    };
  },
};
</script>
