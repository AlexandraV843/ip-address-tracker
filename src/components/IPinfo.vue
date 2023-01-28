<template>
  <header>
    <h3>IP Address Tracker</h3>
    <div class="wrapper">
      <input @keyup.enter="get_IP_name(`http://ip-api.com/json/${ip_name}`)" v-model="ip_name" type="text" aria-label="Default" placeholder="Search for any IP address or domain">
      <button @click="get_IP_name(`http://ip-api.com/json/${ip_name}`)">
          <div></div>
      </button>
    </div>
  </header>
  <main>
    <div class="content__wrapper">
      <div>
        <h3>IP Address</h3>
        <span>{{IP_address}}</span>
      </div>

      <p></p>

      <div>
        <h3>Location</h3>
        <span>{{cityName}}</span>
      </div>

      <p></p>

      <div>
        <h3>Timezone</h3>
        <span>{{timezone}}</span>
      </div>

      <p></p>
      
      <div>
        <h3>Isp</h3>
        <span>{{Ips}}</span>
      </div>
    </div>
  <div id="map">
    <map-component/> 
  </div>
  </main>
  </template>
  
  <script>
  import L from 'leaflet';
  import 'leaflet/dist/leaflet.css';

  export default {
    name: 'IPinfo',
    components: {

    },
    data() {
      return {
        ip_name: '',
        cityName: 'Montreal',
        IP_address: '24.48.0.1',
        timezone: 'America/Toronto',
        Ips: 'Le Groupe Videotron Ltee',
        centerCoordinates: [51.505, -0.09],
        map: null,
        tileLayer: null
      }
    },
    mounted() {
    this.initMap();
    },
    methods: {
      get_IP_name(ip) {
        fetch(ip).then(res => res.json()).then(data => {
          this.cityName = data.city;
          this.Ips = data.isp;
          this.IP_address = data.query;
          this.timezone = data.timezone;
          this.map.setView(new L.LatLng(data.lat, data.lon), 15);
         
        })
      },
      initMap() {
      this.map = L.map('map').setView(this.centerCoordinates, 15);
      this.tileLayer = L.tileLayer(
        "https://api.mapbox.com/styles/v1/{id}/tiles/{z}/{x}/{y}?access_token={accessToken}",
        {
          attribution: 'Map data (c) <a href="https://www.openstreetmap.org/">OpenStreetMap</a> contributors, <a href="https://creativecommons.org/licenses/by-sa/2.0/">CC-BY-SA</a>, Imagery (c) <a href="https://www.mapbox.com/">Mapbox</a>',
          maxZoom: 20,
          id: "mapbox/streets-v11",
          accessToken:"sk.eyJ1IjoiYWxleDEzNTkwIiwiYSI6ImNsOXJqazJlbzBxa3IzbnRmZjB3eGQ2MDAifQ.1wO5iDENzFPgp3pNpuNJ-A",
        }
      );
      this.tileLayer.addTo(this.map);
      },
    }
  }
  </script>

<style scoped>
* {
  margin: 0;
  padding: 0;

}

header {
  background-image: url(./images/pattern-bg.png);
  height: 220px;
}

 header h3 {
  color: white;
  padding-top: 40px;
  padding-bottom: 10px;
}

main {
  position: relative;
  display: flex;
  flex-direction: column;
  width: 100%;
  z-index: 3;
}


button div {
    border: solid white;
    border-width: 0 1.5px 1.5px 0;
    display: inline-block;
    padding: 3px;
    transform: rotate(-45deg);
}

main h3 {
  color: #666a73;
  font-size: 13px;
  font-weight: 900;
}

main span {
  font-weight: 900;
}

main p {
  width: 1px;
  height: 55px;
  background: #d3d9e8;
  margin-top: 25px;
}
.content__wrapper {
  display: flex;
  justify-content: center;
  width: 90%;
  align-self: center;
  border-radius: 12px;
  box-shadow: -2px 5px 20px 9px rgba(34, 60, 80, 0.2);
  margin-top: -60px;
  background: white;
  position: absolute;
  z-index: 3;
  min-width: 560px;
  max-width: 800px;
}
.content__wrapper div {
  margin: 30px;
}

input {
  outline: none;
  border-radius: 10px 0 0 10px;
  max-width: 500px;
  border: none;
  width: 48vw;
  height: 40px;
  padding-left: 15px;
  
}

button {
  background: black;
  width: 40px;
  height: 40px;
  border: none;
  border-radius: 0 5px 5px 0;

}

.wrapper {
  display: flex;
  justify-content: center;
}

#map{
  width: 100%;
  z-index: 1;
  position: absolute;
  height: 73vh;
}

@media screen and (max-width: 780px) {
  .content__wrapper div {
    margin-right: 20px;
  }

  main p {
  width: 1.5px;
  }
}

@media screen and (max-width: 635px) {
  .content__wrapper {
    display: flex;
    flex-direction: column;
    min-width: 160px;
    width: 55%;
    height: 300px;
    padding-top: 30px;
    padding-bottom: 30px;
  }

  .content__wrapper div {
    margin: 0;
  }

}


</style>

  