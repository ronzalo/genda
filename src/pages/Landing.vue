<template>
    <div class="container">
        <Navbar />
        <section class="hero is-primary">
            <div class="hero-body">
                <div class="container">
                    <h1 class="title">
                        Primary title
                    </h1>
                    <h2 class="subtitle">
                        Primary subtitle
                    </h2>
                </div>
            </div>
        </section>
        <div class="columns">
            <div class="column is-5">
                <nav class="panel">
                    <p class="panel-heading">
                        Información y ubicación
                    </p>
                    <div class="panel-block">
                        <div id="mapid"></div>
                    </div>
                </nav>
            </div>
            <div class="column is-4">
                <nav class="panel">
                    <p class="panel-heading">
                        Seleccione el local
                    </p>
                    <div class="panel-block">
                        <div class="list">
                            <div v-for="store in stores" @click="updateCurrentStore(store)" :key="store.name" class="list-item">
                                <strong>{{ store.name }}</strong>
                                <p>{{ store.address }}</p>
                                <p>{{ store.phone }}</p>
                            </div>
                        </div>
                    </div>
                </nav>
            </div>

            <div class="column is-3">
                <nav class="panel">
                    <p class="panel-heading">
                        Horario
                    </p>
                    <div class="panel-block">
                        <button class="button">Agendar</button>
                    </div>
                </nav>
            </div>
        </div>

    </div>
</template>

<script>
import Navbar from "@/components/Navbar";
import Steps from "@/components/Steps";
import sourceData from "@/data";

export default {
  components: { Navbar, Steps },
  data() {
    return {
      stores: sourceData.stores,
      currentStore: sourceData.stores[0]
    };
  },

  methods: {
      updateCurrentStore (store) {
          return this.currentStore = store
      }
  },

  mounted() {
    let coords = this.currentStore.coords;
    const mymap = L.map("mapid").setView([coords.lat, coords.lng], 15);
    L.tileLayer(
      "https://api.tiles.mapbox.com/v4/{id}/{z}/{x}/{y}.png?access_token={accessToken}",
      {
        attribution:
          'Map data &copy; <a href="https://www.openstreetmap.org/">OpenStreetMap</a> contributors, <a href="https://creativecommons.org/licenses/by-sa/2.0/">CC-BY-SA</a>, Imagery © <a href="https://www.mapbox.com/">Mapbox</a>',
        maxZoom: 18,
        id: "mapbox.streets",
        accessToken:
          "pk.eyJ1Ijoicm9ja3phbG8iLCJhIjoiY2pvb252cDFyMG92OTNwbXNzZHJiamM1biJ9.RXUWFQMrFrlNvaRe7FmVZQ"
      }
    ).addTo(mymap);

    L.marker([coords.lat, coords.lng]).addTo(mymap);
  }
};
</script>

<style>
#mapid {
  height: 300px;
  flex: 1;
}
</style>