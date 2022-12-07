<template>
  <div ref="map-root"
       style="width: 100%; height: 100%">
  </div>
</template>

<script>
  import View from 'ol/View'
  import Map from 'ol/Map'
  import TileLayer from 'ol/layer/Tile'
  import OSM from 'ol/source/OSM'

  const url = "http://localhost:8081/geoserver/pervasif/wfs?service=WFS&version=1.1.0.0&request=GetFeature&typename=pervasif:pervasif_ressources&outputFormat=application/json";
  
  fetch(url)
    .then(response => response.json())
    .then(data => {
      // Traiter les données ici
      console.log(data);
    })
    .catch(error => {
      // Gérer les erreurs ici
      console.error(error);
    });

  // importing the OpenLayers stylesheet is required for having
  // good looking buttons!
  import 'ol/ol.css'
  import VectorLayer from 'ol/layer/Vector'
  import VectorSource from 'ol/source/Vector'
  import GeoJSON from 'ol/format/GeoJSON'
  // this is a simple triangle over the atlantic ocean
  const data = {
    type: 'Feature',
    properties: {},
    geometry: {
      type: 'Polygon',
      coordinates: [
        [
          [
            46.32206501599309, -0.4587208979120951
          ],
          [
            -28.125,
            23.563987128451217
          ],
          [
            -10.8984375,
            32.84267363195431
          ],
          [
            46.32206501599309, -0.4587208979120951
          ]
        ]
      ]
    }
  };

  export default {
    name: 'MapContainer',
    components: {},
    props: {},
    mounted() {
      // a feature (geospatial object) is created from the GeoJSON
      const feature = new GeoJSON().readFeature(data, {
        // this is required since GeoJSON uses latitude/longitude,
        // but the map is rendered using “Web Mercator”
        featureProjection: 'EPSG:3857'
      });

      // a new vector layer is created with the feature
      const vectorLayer = new VectorLayer({
        source: new VectorSource({
          features: [feature],
        }),
      })
      // this is where we create the OpenLayers map
      new Map({
        // the map will be created using the 'map-root' ref
        target: this.$refs['map-root'],
        layers: [
          // adding a background tiled layer
          new TileLayer({
            source: new OSM() // tiles are served by OpenStreetMap
          }),
          vectorLayer
        ],

        // the map view will initially show the whole world
        view: new View({
          zoom: 18,
          center: [-51055.754745, 5832239.012951],
          constrainResolution: true
        }),
      })
    },
  }
</script>