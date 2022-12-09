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

  // importing the OpenLayers stylesheet is required for having
  // good looking buttons!
  import 'ol/ol.css'
  import VectorLayer from 'ol/layer/Vector'
  import VectorSource from 'ol/source/Vector'
  import Feature from 'ol/Feature';
  import {Point} from "ol/geom";
  // this is a simple triangle over the atlantic ocean

  const url = "http://localhost:8081/geoserver/wfs?service=wfs&version=2.0.0&request=GetFeature&typeNames=pervasif:ressources&outputFormat=application/json";

  // const place = [-51055.754745, 5832250.012951];
  // const point = new Point(place);

  var places = []

  fetch(url)
    .then(response => response.json())
    .then(data => {
      data['features'].forEach(element => {
        places.push(new Point(element['geometry']['coordinates']))
      });
    })
    .catch(error => {
      console.error("Erreur : " + error);
    });



  export default {
    name: 'MapContainer',
    components: {},
    props: {},
    mounted() {

      const vectorSource = new VectorSource();

      const features = places.map(place => new Feature(new Point(place)));
      vectorSource.addFeatures(features);




      new Map({
        // the map will be created using the 'map-root' ref
        target: this.$refs['map-root'],
        layers: [
          // adding a background tiled layer
          new TileLayer({
            source: new OSM() // tiles are served by OpenStreetMap
          }),
          new VectorLayer({
            source: vectorSource,
            style: {
              'circle-radius': 150,
              'circle-fill-color': 'black',
            }
          })
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