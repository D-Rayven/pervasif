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



const place = [-51055.754745, 5832250.012951];
const point = new Point(place);



export default {
  name: 'MapContainer',
  components: {},
  props: {},
  mounted() {

    // a new vector layer is created with the feature


    // this is where we create the OpenLayers map
    new Map({
      // the map will be created using the 'map-root' ref
      target: this.$refs['map-root'],
      layers: [
        // adding a background tiled layer
        new TileLayer({
          source: new OSM() // tiles are served by OpenStreetMap
        }),
        new VectorLayer({
          source: new VectorSource({
            features: [new Feature(
                point
            )],
            style: {
              'circle-radius': 150,
              'circle-fill-color': 'black',
            },
          }),
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