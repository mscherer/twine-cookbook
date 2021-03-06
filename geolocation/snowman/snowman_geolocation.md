# "Geolocation": Snowman (v1.3.0)

## Summary

Many browsers allow access to the current location through the [Geolocation property](https://developer.mozilla.org/en-US/docs/Web/API/Geolocation) and associated functions. This functionality is subject to the user agreeing to allow access. Until the functionality is unlocked, or if the user declines, default values will be returned.

Functionality availability and their results should always be tested against other location services or information. Most browsers will return results through the fastest and sometimes least-accurate methods possible.

This example uses [Underscore template functionality](http://underscorejs.org/#template) to test for and show the values of properties storied in the ```s``` global variable in Snowman.


## Live Example

<section>
<iframe src="snowman_geolocation_example.html" height=400 width=90%></iframe>


Download: <a href="snowman_geolocation_example.html" target="_blank">Live Example</a>
</section>

## Twee Code

```
:: StoryTitle
Geolocation in Snowman

:: UserScript[script]
(function () {

	window.geolocation = {

		available: function() {
			return ("geolocation" in navigator 
				&& typeof navigator.geolocation.getCurrentPosition === "function");
		},
		getLocation: function() {

			// Create initial values
			var location = { latitude : 0, longitude : 0 };
			
			// Create success callback to store values
			var	positionSuccess = function (position) {
				
				location.latitude = position.coords.latitude;
				location.longitude = position.coords.longitude;

			};
			
			// Create error callback
			var positionError = function (error) {
				/* Code that handles errors */
			};
			
			// Create initial options
			var positionOptions = {
				timeout: 31000, 
				enableHighAccuracy: true,
				maximumAge : 120000
			};
			

			// Ask for location based on callbacks and options
			navigator.geolocation.getCurrentPosition(
				positionSuccess,
				positionError,
				positionOptions
			);

			// Return location found
			// If not location, will return initial (0,0) values
			return location;

		},
		approximateLocation: function (a, b, allowedDiff) { 
		    // allowedDiff must always be > 0
			if (a === b) { // handles various "exact" edge cases
				return true;
			}

			allowedDiff = allowedDiff || 0.0005;
			
			return Math.abs(a - b) < allowedDiff;
		}

	};
	
}());

:: Start
[[Ask for permission]]

:: Show results
Is geolocation available? <%= window.geolocation.available() %>
<%
	if(window.geolocation.available()) {
		s.location = window.geolocation.getLocation();
	}
%>

If so, what is the current location?

Latitude: <%= s.location.latitude %>

Longitude: <%= s.location.longitude %>

Are we in the approximate location of Stonehenge (51.1788853, -1.828409)?

Latitude: <%= window.geolocation.approximateLocation(s.location.latitude, 51.1788853) %>

Longitude: <%= window.geolocation.approximateLocation(s.location.longitude, -1.828409) %>

:: Ask for permission
<%
	if(window.geolocation.available()) {
		s.location = window.geolocation.getLocation();
	}
%>
[[Show results]]

```

Download: <a href="snowman_geolocation_twee.txt" target="_blank">Twee Code</a>
