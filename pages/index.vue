<template>
  <div>
    
  <div id="map">
    <MglMap :accessToken="accessToken" :mapStyle="mapStyle" :center="center" @load="maploaded" :zoom="zoom" :hash="true">
      <!-- <MglAttributionControl /> -->
      <MglNavigationControl position="top-right" />
      <MglGeolocateControl position="top-right" />
      <!-- <MglScaleControl /> -->
      <MglGeojsonLayer
        v-if="pshow"
        :sourceId="geoJsonSource.data.id"
        :source="geoJsonSource"
        :layerId="geoJsonLayer.id"
        :layer="geoJsonLayer"
        :clearSource="false"
      />
      <MglGeojsonLayer
        v-if="lshow"
        :sourceId="geoJsonSource2.data.id"
        :source="geoJsonSource2"
        :layerId="geoJsonLayer2.id"
        :layer="geoJsonLayer2"
        :clearSource="false"
      />
    </MglMap>
    <div class="tog">
      <h4 style="font-weight:bold"> Click to toggle </h4>
    <input type=checkbox @click="pshow=!pshow" id="pswitch" name="Points"> Points <br>
    <input type=checkbox @click="lshow=!lshow" id="pswitch" name="Lines"> Lines <br>
    </div>
    <!-- <div class="tog">
      <h4 style="font-weight:bold"> Click to toggle </h4>
    <input type=radio @click="mapStyle='mapbox://styles/daiyaanmapbox/ckfxn26sp0efl19lgp69obgg8'" id="sswitch" name="style"> Style 1 <br>
    <input type=radio @click="mapStyle='mapbox://styles/mapbox/streets-v11'" id="sswicth" name="style"> Style 2 <br>
    </div> -->
    <div id="menu">
        <input
        id="streets-v11"
        type="radio"
        name="rtoggle"
        value="streets"
        checked="checked"
        />
        <label for="streets-v11">streets</label>
        <input id="light-v10" type="radio" name="rtoggle" value="light" />
        <label for="light-v10">light</label>
        <input id="dark-v10" type="radio" name="rtoggle" value="dark" />
        <label for="dark-v10">dark</label>
        <input id="outdoors-v11" type="radio" name="rtoggle" value="outdoors" />
        <label for="outdoors-v11">outdoors</label>
        <input id="satellite-v9" type="radio" name="rtoggle" value="satellite" />
        <label for="satellite-v9">satellite</label>
    </div>
  </div>
  </div>
</template>

<script>
import { MglMap, MglGeojsonLayer, MglNavigationControl, MglAttributionControl,
    MglFullscreenControl, MglGeolocateControl  } from "v-mapbox"
import mapboxGl from 'mapbox-gl';

export default {
  components: {
    MglMap,
    MglGeojsonLayer,
    MglNavigationControl,
    MglGeolocateControl,
  },
  data() {
    return {
      accessToken: process.env.mapToken, // your access token.
      pshow: true,
      mpx:null,
      lshow: true,
      // mapStyle: "mapbox://styles/daiyaanmapbox/ckfxn26sp0efl19lgp69obgg8", // your map style
      mapStyle:"mapbox://styles/mapbox/streets-v11",
      center: [78.046875,20.138470312451155],
      zoom: 3,
      showPath: true,
      geoJsonSource: {
        type: "geojson",
        data: {
          id: "points",
          type: "FeatureCollection",
          features: [
            {
              type: "Feature",
              properties: {},
              geometry: {
                type: "Point",
                coordinates: [83.671875,32.24997445586331]
              }
            },
            {
              type: "Feature",
              properties: {},
              geometry: {
                type: "Point",
                coordinates: [77.16796875,37.579412513438385]
              }
            }
          ]
        }
      },
      geoJsonSource2: {
        type: "geojson",
        data: {
          id: "lines",
          type: "FeatureCollection",
          features: [
            {
              type: "Feature",
              properties: {},
              geometry: {
                type: "LineString",
                coordinates: [
                        [
                            73.7841796875,
                            22.836945920943855
                         ],
                        [
                            79.0576171875,
                            16.551961721972525
                        ]
                ]
              }
            },
            {
              type: "Feature",
              properties: {},
              geometry: {
                type: "LineString",
                coordinates: [      
                            [
                                73.4326171875,
                                26.15543796871355
                            ],
                            [
                                85.341796875,
                                21.657428197370653
                            ]
                        ]
              }
            }
          ]
        }
      },
      geoJsonLayer: {
        id: "points",
        type: "circle",
        paint: {
          "circle-radius": 10,
          "circle-color": "red",
          "circle-stroke-width": 2,
          "circle-stroke-color": "#000000"
        }
      },
      geoJsonLayer2: {
        id: "lines",
       type:'line',
      'layout': {
                  'line-cap': 'round',
                  'line-join': 'round',
                },
      'paint': {
                 'line-color': 'red',
                 'line-width': 5,
                 'line-opacity': 0.8
               }
      }
    };
  },
  // mounted(){
  //   const map=mapboxGl.MglMap
  //   var layerList = document.getElementById('menu');
  //   var inputs = layerList.getElementsByTagName('input');
    
  //   function switchLayer(layer) {
  //   var layerId = layer.target.id;
  //   this.mapStyle='mapbox://styles/mapbox/' + layerId;
  //   }
  //   for (var i = 0; i < inputs.length; i++) {
  //   inputs[i].onclick = switchLayer;
  //   }
  // },
  methods:{
    maploaded ({map,component}) { 
      const mp =map
      console.log(mp)
      // console.log(this.pshow)
        var customLayers = [{
        source: {
            type: "geojson",
            data: {
                id: "points",
                type: "FeatureCollection",
                features: [
                  {
                    type: "Feature",
                    properties: {},
                    geometry: {
                      type: "Point",
                      coordinates: [83.671875,32.24997445586331]
                    }
                  },
                  {
                    type: "Feature",
                    properties: {},
                    geometry: {
                      type: "Point",
                      coordinates: [77.16796875,37.579412513438385]
              }
            }
          ]
        }
        },
        layer: {
            id: "points",
        source: "points",
        type: "circle",
        paint: {
          "circle-radius": 10,
          "circle-color": "red",
          "circle-stroke-width": 2,
          "circle-stroke-color": "#000000"
        }
        }
    },
    {
      source:{
        type: "geojson",
        data: {
          id: "lines",
          type: "FeatureCollection",
          features: [
            {
              type: "Feature",
              properties: {},
              geometry: {
                type: "LineString",
                coordinates: [
                        [
                            73.7841796875,
                            22.836945920943855
                         ],
                        [
                            79.0576171875,
                            16.551961721972525
                        ]
                ]
              }
            },
            {
              type: "Feature",
              properties: {},
              geometry: {
                type: "LineString",
                coordinates: [      
                            [
                                73.4326171875,
                                26.15543796871355
                            ],
                            [
                                85.341796875,
                                21.657428197370653
                            ]
                        ]
              }
            }
          ]
        }
      },
      layer:{
      id: "lines",
      source:"lines",
      type:'line',
      'layout': {
                  'line-cap': 'round',
                  'line-join': 'round',
                },
      'paint': {
                 'line-color': 'red',
                 'line-width': 5,
                 'line-opacity': 0.8
               }
      }

    }]
    const that=this
    map.on('style.load', function() {
        // Ading custom soruces and layers after a style change
        
        for (var i = 0; i < customLayers.length; i++) {
            var lay = customLayers[i]
            if(i==0 && that.pshow==true || i==1 && that.lshow==true){
               map.addSource(lay.layer.source, lay.source);
               map.addLayer(lay.layer)
            }
           
        }
    });

    var layerList = document.getElementById('menu');
    var inputs = layerList.getElementsByTagName('input');

    function switchLayer(layer) {
        var layerId = layer.target.id;
        map.setStyle('mapbox://styles/mapbox/' + layerId);
    }

    for (var i = 0; i < inputs.length; i++) {
        inputs[i].onclick = switchLayer;

    }
  },

}
}
</script>

<style>
.mapboxgl-map{
  height:75vh !important;
}
.tog{
  width:200px;
  margin:20px 40px;
  border: 2px solid navy;
  padding:0px 15px 10px 15px;;
  border-radius:5px;
  float:left;
  margin-left:50%;
}
#menu {
position: absolute;
background: #fff;
padding: 10px;
font-family: 'Open Sans', sans-serif;
z-index: 100;
}
</style>