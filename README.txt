<!DOCTYPE html>
<html>  
  <head>
    <!-- Metadata -->  
    <title>Manuel GP | BigData</title>
    <meta name="viewport" content="initial-scale=1.0, user-scalable=no" />
    <meta http-equiv="content-type" content="text/html; charset=UTF-8"/>
    <link rel="shortcut icon" href="http://www.iconarchive.com/download/i83643/pelfusion/long-shadow-media/Maps-Pin-Place.ico" />
    <!-- CSS -->
    <style>
      html, body, #map {
        height: 100%;
        padding: 0;
        margin: 0;
      }
      #layer_selector {
        position: absolute;
        top: 60px;
        right: 20px;
        padding: 0;
      }
      #layer_selector ul {
        padding: 0; margin: 0;
        list-style-type: none;
      }
      #layer_selector li {
        border-bottom: 1px solid #999;
        padding: 15px 30px;
        font-family: "Helvetica", Arial;
        font-size: 13px;
        color: #444;
        cursor: auto;
      }
      #layer_selector li:hover {
        background-color: #F0F0F0;
        cursor: pointer;
      }
      #layer_selector li.selected {
        background-color: #EEE;
      }
    </style>

    <link rel="stylesheet" href="http://libs.cartocdn.com/cartodb.js/v3/3.15/themes/css/cartodb.css" />
    
    <!-- JavaScript library -->
    <script src="http://libs.cartocdn.com/cartodb.js/v3/3.15/cartodb.js"></script>
    
  </head>
  
  <body>
    <!-- Visual Map -->  
    <div id="map"></div>
    <div><iframe style="position: absolute; bottom: 30px; right: 20px; padding: 0;" width=300 height=200 frameborder="0" src="https://manugp1994.carto.com/builder/7cb8b843-8905-499b-aebd-11a6191e9d88/embed" allowfullscreen webkitallowfullscreen mozallowfullscreen oallowfullscreen msallowfullscreen></iframe></div>
    <div id="layer_selector" class="cartodb-infobox">
      <ul>
        <li data="all" class="selected">All earthquake</li>
        <li data="2">magnitude > 2</li>
        <li data="3">magnitude > 3</li>
        <li data="4">magnitude > 4</li>
        <li data="5">magnitude > 5</li>
      </ul>
    </div>
    
    <!-- Functions -->
    <script>
      // create layer selector
      
      function createSelector(layer) {
        var sql = new cartodb.SQL({ user: 'user' });
        var $options = $('#layer_selector li');
        
        $options.click(function(e) {
          // get the mag of the selected layer
          var $li = $(e.target);
          var area = $li.attr('data');
          // deselect all and select the clicked one
          $options.removeClass('selected');
          $li.addClass('selected');
          // create query based on data from the layer
          var query = "SELECT * FROM table_2_5_month";
          if(area !== 'all') {
            query = "SELECT * FROM table_2_5_month WHERE mag > " + area;
          }
          // change the query in the layer to update the map
          layer.setSQL(query);
        });
      }
      
      function main() {
        cartodb.createVis('map', 'http://manugp1994.carto.com/api/v2/viz/3f08b071-5003-4bde-963b-d8b74c9ac33d/viz.json', {
          tiles_loader: true,
          //center_lat: 50,
          //center_lon: 20,
          legends: true,
          zoom: 2
        })
        .done(function(vis, layers) {
          // layer 0 is the base layer, layer 1 is cartodb layer
          var subLayer = layers[1].getSubLayer(0);
          createSelector(subLayer);
        })
        .error(function(err) {
          console.log(err);
        });
      }
      
      window.onload = main;
    </script>
    
    
  </body>
</html>
