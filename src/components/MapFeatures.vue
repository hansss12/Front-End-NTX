<template>
  <div
    class="w-full md:w-auto absolute md:top-[40px] md:left-[60px] z-[2] flex gap-4 px-6 py-8 md:px-0 md:py-0 bg-transparent"
  >
    <!-- User Location -->
    <div
      class="px-4 bg-white flex items-center shadow-md rounded-md min-h-[45px] cursor-pointer"
      @click="$emit('getGeolocation')"
      :class="{ 'bg-slate-600': coords }"
    >
      <i
        class="fas fa-location-arrow 'text-slate-600' text-[18px]"
        :class="{ 'text-white': coords, 'animate-pulse': fetchCoords }"
      ></i>
    </div>

    <!-- attack location -->
    <div
      class="px-4 bg-white flex items-center shadow-md rounded-md min-h-[45px] cursor-pointer"
      @click="$emit('getAttacklocation')"
      :class="{ 'bg-slate-600': attackCoords }"
    >
      <i
        class="fas fa-map-marker-alt 'text-slate-600' text-[18px]"
        :class="{ 'text-white': attackCoords, 'animate-pulse': fetchAttackCoords }"
      ></i>
    </div>
    
  </div>
</template>

<script>
export default {
  props: ["fetchCoords", "coords", "fetchAttackCoords", "attackCoords"],
};
</script>
