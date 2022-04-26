---
layout: about-page
title: Data Streams
permalink: /data-streams/
---
 <link rel="stylesheet" href="https://maxcdn.bootstrapcdn.com/bootstrap/3.4.1/css/bootstrap.min.css">
<style> 
.centered{
    margin: 0 auto;
    width: 200px;
}
.nav {
    position: relative !important;
    z-index: 999;
    display: block;
    border:none !important;
}
/* iframe */

.iframe {
  border: 1px solid #131C28;
  overflow: hidden;
  background: #fff;
}

.iframe iframe ,
.iframe img {
  width: 100%;
  height: 200px;
  border: 0;
  display: block;
}


.iframe-header {
  display: none;
}

.js .iframe-header {
  display: block;
}

.iframe-content {
  /* ipad iframe hack */
  height: 400px;
  overflow: auto;
  -webkit-overflow-scrolling: touch;
}

.iframe-header a {
  font-size: 15px;
  color: white;
  background: #3B4658;
  display: block;
  padding: 15px;
  text-align: center;
  border-bottom: 3px solid #131C28;
}

.iframe-header a:hover,
.iframe-header a:focus {
  background: #6A798E;
}

.iframe-full-screen .iframe-header {
  display: block;
  position: absolute;
  height: 50px;
  width: 100%;
}

.iframe-full-screen .iframe-content {
  position: absolute;
  top: 50px;
  bottom: 0;
  width: 100%;
  height: auto;
}

.iframe-full-screen .iframe-header a {
  padding: 0;
  height: 44px;
  line-height: 44px;
  text-align: center;
  border: 3px solid #131C28;
}

.iframe-full-screen body {
  width: 100%;
  height: 100%;
  overflow: hidden;
}

.iframe-full-screen .iframe.iframe-active{
  width: 100%;
  height: 100%;
  position: fixed;
  left: 0;
  top: 0;
  bottom: 0;
  right: 0;
  z-index: 9999;
  border: none;
}

.iframe-full-screen .iframe iframe {
  position: absolute;
  height: 100%;
  width: 100%;
  border: none;
}

.wrapper {
  max-width: 1000px;
  margin: 20px auto;
  padding: 0 20px;
  display: flex;
  flex-wrap: wrap
}

.item {
    display: inline-block;
    flex: 1 300px;
}

@media all and (max-height: 400px){
  .iframe {
    height: 300px;
  }
}

  
</style>
<style>
body {
margin: 0;
padding: 0;
}
#map {
position: relative;

width: 100%;
height:100vh;
}
.marker {
background-image: url('/assets/images/mapbox-icon.png');
background-size: cover;
width: 50px;
height: 50px;
border-radius: 50%;
cursor: pointer;
}
.mapboxgl-popup {
max-width: 200px;
}
.mapboxgl-popup-content {
text-align: center;
font-family: 'Open Sans', sans-serif;
}
</style>


<div class="main">
  <h2>Weather Cam API</h2>
 

  <ul class="nav nav-tabs">
    <li class="active"><a data-toggle="tab" href="#home">Weather Map</a></li>
    <li><a data-toggle="tab" href="#menu1">Weather Grid</a></li>
  
  </ul>

  <div class="tab-content">
    <div id="home" class="tab-pane fade in active">
    <h3>Weather Map</h3>
     <div id="map"></div>
    </div>
    <div id="menu1" style="width:100%" class="tab-pane fade">
      <div id="api" class="wrapper"></div>
     
      </div>
  
  </div>
</div>


  <script src="https://ajax.googleapis.com/ajax/libs/jquery/3.6.0/jquery.min.js"></script>
  <script src="https://maxcdn.bootstrapcdn.com/bootstrap/3.4.1/js/bootstrap.min.js"></script>
 

<link
href="https://fonts.googleapis.com/css?family=Open+Sans"
rel="stylesheet"
/>
<script src="https://api.tiles.mapbox.com/mapbox-gl-js/v2.8.1/mapbox-gl.js"></script>
<link
href="https://api.tiles.mapbox.com/mapbox-gl-js/v2.8.1/mapbox-gl.css"
rel="stylesheet"
/>

<!--<script src="https://cdnjs.cloudflare.com/ajax/libs/jquery/3.3.1/jquery.min.js"></script>-->
<script> 
var url="https://api.oceandrivers.com:443/v1.0/getWebCams/";
console.log(url);

$.ajax({
    url: url,
    method: "get",
    success: function (response) {
        console.log(response);
        //$("#api").text(JSON.stringify(response));
        var html = "";

		var gsonarray=[];
		var myarr=[];
        for(x = 0; x < response.length; x++) {
            var entry = response[x];
            var linktext = entry["url"];
            var extn = entry["url"].split(".");
            console.log(extn);

            var line= "<div class='item'><div class='iframe'>";
            if(extn[3] =="html")
            line += "<iframe src='"+linktext+"' />";
            else 
            line += "<img src='"+linktext+"' />";
			gsonarray= {"type":'',
			"geometry": {"type":'',coordinates:["",""]},
			"properties": {"title":'',"description":""}
			
			
			};

			gsonarray["type"]='Feature';
			gsonarray["geometry"]["type"]="Point";
			gsonarray["geometry"]["coordinates"]=[entry['latitude'],entry['longitude']];
			gsonarray["properties"]["title"]="weathermap";
			gsonarray["properties"]["description"]=entry['name'];
			myarr[x]=gsonarray;

                line+= "</div></div>";
             //line = "<a href='" + entry["url"] + "'>" + linktext + "</a>" + "<br>"
            html += line;
        }
		
		 createmap(myarr) ;
		
        $("#api").html(html);
    }
})

</script>
<script>
mapboxgl.accessToken = 'pk.eyJ1IjoicmFqaGVlcyIsImEiOiJjbDJjMmlvZWkwZHZvM2Rxcmtrc3hhdWkxIn0.A0kU0j0LqpcFiFb3P83jeQ';
 


 function createmap(garray) {
 let geojson = {
'type': 'FeatureCollection',
'features': 
garray
 


};
console.log(geojson);
 
const map = new mapboxgl.Map({
container: 'map',
style: 'mapbox://styles/mapbox/light-v10',
center: [39.7850174,3.132144],
zoom: 8
});
 
// add markers to map
for (const feature of geojson.features) {
// create a HTML element for each feature
const el = document.createElement('div');
el.className = 'marker';
 
// make a marker for each feature and add it to the map
new mapboxgl.Marker(el)
.setLngLat(feature.geometry.coordinates)
.setPopup(
new mapboxgl.Popup({ offset: 25 }) // add popups
.setHTML(
`<h3>${feature.properties.title}</h3><p>${feature.properties.description}</p>`
)
)
.addTo(map);
}
 }
</script>


