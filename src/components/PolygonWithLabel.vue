<template>
  <div>
    <div class="header-margins">
      Google Maps Polygon With Label
      <a target="_blank" href="nightprogrammer.com">nightprogrammer.com</a>
    </div>
    <div id="polygon-label-map" class="map-margins"></div>
  </div>
</template>

<script>
import loadGoogleMapsApi from "load-google-maps-api";
import PolygonLabelComponent from "./PolygonLabel";
import { gMapsApiKey } from "./../constants";
import Vue from "vue";

const PolygonLabelConstructor = Vue.extend(PolygonLabelComponent);

export default {
  name: "AnimatedMarkerMap",
  data() {
    return {
      map: null,
    };
  },
  mounted() {
    loadGoogleMapsApi({
      key: gMapsApiKey,
      libraries: ["places", "drawing", "geometry"],
    }).then(async () => {
      const mapZoom = 12;
      const { google } = window;
      const mapOptions = {
        zoom: mapZoom,
        mapTypeId: google.maps.MapTypeId.HYBRID,
        center: new google.maps.LatLng({ lat: 23, lng: 57 }),
        mapTypeControl: true,
        streetViewControl: false,
        mapTypeControlOptions: {
          position: google.maps.ControlPosition.BOTTOM_LEFT,
        },
      };
      this.map = new google.maps.Map(
        document.getElementById("polygon-label-map"),
        mapOptions
      );

      const polygonLabelComponent = new PolygonLabelConstructor({
        propsData: { fieldName: "Bermuda" },
      });

      polygonLabelComponent.$mount();

      const polygonLabelString = new XMLSerializer().serializeToString(
        polygonLabelComponent.$el
      );

      const polygonLabelUrl =
        "data:image/svg+xml;charset=UTF-8;base64," + btoa(polygonLabelString);

      const polygonCoords = [
        { lat: 25.774, lng: -80.19 },
        { lat: 18.466, lng: -66.118 },
        { lat: 32.321, lng: -64.757 },
        { lat: 25.774, lng: -80.19 },
      ];

      const tempBounds = new google.maps.LatLngBounds();

      for (let j = 0; j < polygonCoords.length; j++) {
        const x = {
          lat: polygonCoords[j].lat,
          lng: polygonCoords[j].lng,
        };
        const BoundLatLng = new google.maps.LatLng({
          lat: parseFloat(x.lat),
          lng: parseFloat(x.lng),
        });
        tempBounds.extend(BoundLatLng);
      }
      const centroid = tempBounds.getCenter();

      const markerLabel = new google.maps.Marker({
        position: centroid,
        icon: polygonLabelUrl,
      });

      const PolygonShape = new google.maps.Polygon({
        paths: polygonCoords,
        strokeColor: "#ffffff",
        map: this.map,
        strokeWeight: 3,
        fillColor: "#00ff00",
        fillOpacity: 0.2,
        zIndex: 99999,
      });

      markerLabel.setMap(this.map);
      PolygonShape.setMap(this.map);
      this.map.setCenter(centroid);
      this.map.setZoom(4);
    });
  },
};
</script>

<style scoped>
.header-margins {
  margin-left: 40px;
  margin-top: 20px;
}

.map-margins {
  height: 400px;
  width: 600px;
  margin: 30px 40px;
}
</style>