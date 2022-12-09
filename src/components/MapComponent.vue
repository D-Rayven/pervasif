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

  import 'ol/ol.css'
  import VectorLayer from 'ol/layer/Vector'
  import VectorSource from 'ol/source/Vector'

  import GeoJSON from 'ol/format/GeoJSON'

  const url = "http://localhost:8081/geoserver/wfs?service=wfs&version=2.0.0&request=GetFeature&typeNames=pervasif:ressources&outputFormat=application/json";
  
  var points = []
  fetch(url)
    .then(response => response.json())
    .then(data => {
      data['features'].forEach(element => {
        points.push(element['geometry']['coordinates'])
      });
      console.log(points)
    })
    .catch(error => {
      console.error("Erreur : " + error);
    });

  const data = {
    type: 'Feature',
    properties: {},
    geometry: {
      type: 'Polygon',
      coordinates: [
        [
          [
          -51070.086687, 5832385.915365
          ],
          [
          -50901.089195, 5832266.482508
          ],
          [
          -51037.242652, 5832113.011288
          ],
          [
          -51070.086687, 5832385.915365
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
      const feature = new GeoJSON().readFeature(data, {
        featureProjection: 'EPSG:3857'
      });

      const vectorLayer = new VectorLayer({
        source: new VectorSource({
          features: [feature],
        }),
      })
      new Map({
        target: this.$refs['map-root'],
        layers: [
          new TileLayer({
            source: new OSM()
          }),
          vectorLayer
        ],

        view: new View({
          zoom: 18,
          center: [-51055.754745, 5832239.012951],
          constrainResolution: true
        }),
      })
    },
  }
</script>