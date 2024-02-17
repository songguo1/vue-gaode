<template>
<div>
  <div>
      <MapView />
  </div>
  <div>
    <Search/>
  </div>
</div>
</template>

<script>
import MapView from "./components/MapView.vue";
import Search from "./components/Search.vue"
export default {
components: {MapView,Search}
}
</script>

<style>

</style>
