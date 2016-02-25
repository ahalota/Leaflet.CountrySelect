# Leaflet.CountrySelect
Leaflet plugin control to select and load countries from [johan's world.geo.json file](https://github.com/johan/world.geo.json).

## [DEMO](http://ahalota.github.io/Leaflet.CountrySelect/demo.html)

## Options

### Title
An optional title for the first entry. Defaults to "Country". Set as an empty string to omit entirely.
```
var select = L.control.countrySelect({title:'Pick a country!'});
```

### Include
An array of country names from list. If defined, ONLY these countries will be loaded into the select menu.
```
var select = L.control.countrySelect({include:southAmericanCountries}).addTo(map);
```

### Exclude
An array of country names to ommit from list. Must exactly match the names used by the control.
```
var select = L.control.countrySelect({exclude:coldCountries}).addTo(map);
```

### Additional Parameters
The country list is stored in L.Control.CountrySelect.countries as a key/value object with key = Country Name and value = GeoJSON feature.
You can replace the country list used to load data by setting
```
L.Control.CountrySelect.countries = {/*new-list-of-countries*/}
```

## Event Listener: Change
This control can listen on a 'change' event. The returned event includes a 'feature' variable, which contains the GeoJSON feature matching the newly selected country. The [demo](http://ahalota.github.io/Leaflet.CountrySelect/demo.html), shown below, adds the selected country as a feature to the map, and removes it once a new entry is selected.
```
var select = L.control.countrySelect().addTo(map);

select.on('change', function(e){
	if(e.feature === undefined){ //No action when the first item ("Country") is selected
		return;
	}
	var country = L.geoJson(e.feature);
	if (this.previousCountry != null){
		map.removeLayer(this.previousCountry);
	}
	this.previousCountry = country;

	map.addLayer(country);
	map.fitBounds(country.getBounds());
	
});
```
(Note: `this.previousCountry` is not a part of the control, just created for this eventHandler to keep track of the prior feature created)
