---
layout: post
title: "Adding google map with location markers in ruby on rails."
author: annu_yadav
modified:
share: true
categories: blog
excerpt:
tags: []
image:
  feature:
date: 2015-08-01T15:32:59+0530
comments: true
---

We can use gmaps4rails gem to add google map in your application.First add gmaps4rails gem in Gemfile and run bundle.

{% highlight ruby %}
gem 'gmaps4rails'
{% endhighlight %}

require gmaps/google in application.js
You also need to add underscore.js in your application.

{% highlight javascript %}
//= require underscore
//= require gmaps/google
{% endhighlight %}

Add these Javascript Dependencies (google scripts) to your html (may be in layout or in some specific file whwere you want google map to be added)

{% highlight html %}
<script src="//maps.google.com/maps/api/js?v=3.13&amp;sensor=false&amp;libraries=geometry" type="text/javascript"></script>
<script src='//google-maps-utility-library-v3.googlecode.com/svn/tags/markerclustererplus/2.0.14/src/markerclusterer_packed.js' type='text/javascript'></script>
{% endhighlight %}

Now add the html and javascript for adding map data.

{% highlight html %}
<div style='width: 800px;'>
  <div id="map" style='width: 800px; height: 400px;'></div>
</div>

<script>
var handler = Gmaps.build('Google');
handler.buildMap({internal: {id: 'map'}, provider: { zoom: 7 }})
</script>
{% endhighlight %}

It will add google map to div with id 'map'.

{% include image.html img="assets/images/default_map.png" title="default map" %}

We can add markers to the map and also add custom image for the marker.
Center of map can be defined by:

{% highlight html %}
<div style='width: 800px;'>
  <div id="map" style='width: 800px; height: 400px;'></div>
</div>

<script>
    var handler = Gmaps.build('Google');
    handler.buildMap({internal: {id: 'map'}, provider: {zoom: 7}}, function () {
        var markers = handler.addMarkers([
            {lat: 28.0, lng: 76.2},
            {lat: 28.38, lng: 77.82},
            {lat: 28.98, lng: 76.19},
            {
                lat: 28.63, lng: 77.13, infowindow: 'Your location',
                picture: {
                    url: 'http://maps.google.com/mapfiles/ms/icons/green-dot.png',
                    width: 36,
                    height: 36
                }
            },
            {lat: 29.0, lng: 78.17},
            {lat: 28.44, lng: 76.48}
        ]);
        handler.bounds.extendWith(markers);
        handler.map.centerOn({lat: 28.63, lng: 77.13})
    });
</script>
{% endhighlight %}

{% include image.html img="assets/images/map_with_markers.png" title="map with markers" %}

For adding a circle for a particular area:

{% highlight html %}
<div style='width: 800px;'>
  <div id="map" style='width: 800px; height: 400px;'></div>
</div>

<script>
    var handler = Gmaps.build('Google');
    handler.buildMap({internal: {id: 'map'}, provider: {zoom: 8}}, function () {
        var markers = handler.addMarkers([
            {lat: 28.0, lng: 76.2},
            {lat: 28.38, lng: 77.82},
            {lat: 28.98, lng: 76.19},
            {
                lat: 28.63, lng: 77.13, infowindow: 'Your location',
                picture: {
                    url: 'http://maps.google.com/mapfiles/ms/icons/green-dot.png',
                    width: 36,
                    height: 36
                }
            },
            {lat: 29.0, lng: 78.17},
            {lat: 28.44, lng: 76.48}
        ]);
        var circles = handler.addCircles(
                [{lat: 28.63, lng: 77.13, radius: 80000}],
                {strokeColor: '#FF0000', strokeOpacity: 0.7, strokeWeight: 1, fillColor: '#FF0000', fillOpacity: 0.15}
        );
        handler.bounds.extendWith(markers);
        handler.bounds.extendWith(circles);
        handler.fitMapToBounds();
        handler.map.centerOn({lat: 28.63, lng: 77.13})
    });
</script>
{% endhighlight %}

{% include image.html img="assets/images/map_with_circle.png" title="map with circle" %}

Same as circle you can also add multiple shapes by:

{% highlight html %}
addCircle(data, options)
addPolyline(data, options)
addPolygon(data, options)
addKml(data, options)
{% endhighlight %}
