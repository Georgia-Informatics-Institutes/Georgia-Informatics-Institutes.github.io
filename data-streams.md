---
layout: about-page
title: Data Streams
permalink: /data-streams/
---

<style> 
.centered{
    margin: 0 auto;
    width: 200px;
}
</style>
<div class="centered"> Weather Cam API </div>
<div class="main">
<div id="api"></div>
</div>

<script src="https://cdnjs.cloudflare.com/ajax/libs/jquery/3.3.1/jquery.min.js"></script>
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
        for(x = 0; x < response.length; x++) {
            var entry = response[x];
            var linktext = entry["url"]
            var line = "<a href='" + entry["url"] + "'>" + linktext + "</a>" + "<br>"
            html += line;
        }
        $("#api").html(html);
    }
})
</script>

