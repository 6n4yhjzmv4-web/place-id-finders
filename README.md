# place-id-finders
⁠Google Maps Place ID Finder
The **Place ID Finder** sample allows a user to find a place based
on its address, then it adds a marker for the place to the map, and displays
the place's place ID in an info window.

Read the [documentation](https://developers.google.com/maps/documentation/javascript/places#placeid).
<iframe src="https://maps-docs-team.web.app/samples/places-placeid-finder/dist/" allow="fullscreen; "></iframe>

### TypeScript

```typescript
// This sample uses the Place Autocomplete widget to allow the user to search
// for and select a place. The sample then displays an info window containing
// the place ID and other information about the place that the user has
// selected.

async function init(): Promise<void> {
    // Request needed libraries.
    const [{ InfoWindow }, { AdvancedMarkerElement }] = await Promise.all([
        google.maps.importLibrary('maps'),
        google.maps.importLibrary('marker'),
        google.maps.importLibrary('places'),
    ]);

    const mapElement = document.querySelector('gmp-map')!;
    const map = mapElement.innerMap;

    const placeAutocomplete = document.querySelector('gmp-place-autocomplete')!;

    // Set the map options.
    map.setOptions({
        clickableIcons: false,
        mapTypeControl: false,
        streetViewControl: false,
    });

    // Use the bounds_changed event to bias results to the current map bounds.
    map.addListener('bounds_changed', () => {
        const bounds = map.getBounds();
        if (bounds) {
            placeAutocomplete.locationBias = bounds;
        }
    });

    const infoWindow = new InfoWindow();
    const infoWindowContent = document.getElementById('infowindow-content')!;

    infoWindow.setContent(infoWindowContent);

    const marker = new AdvancedMarkerElement({
        map,
        collisionBehavior: 'REQUIRED_AND_HIDES_OPTIONAL',
        gmpClickable: true,
    });

    marker.addEventListener('gmp-click', () => {
        infoWindow.open(map, marker);
    });

    placeAutocomplete.addEventListener(
        'gmp-select',
        async ({ placePrediction }) => {
            infoWindow.close();

            const place = placePrediction.toPlace();

            await place.fetchFields({
                fields: ['displayName', 'formattedAddress', 'location', 'id'],
            });

            if (!place.location) {
                return;
            }

            if (place.viewport) {
                map.fitBounds(place.viewport);
            } else {
                map.setCenter(place.location);
                map.setZoom(17);
            }

            // Set the position of the marker using the place ID and location.
            marker.position = place.location;
            // marker.setVisible(true); // AdvancedMarkerElement is visible by default when map and position are set.

            infoWindowContent.children.namedItem('place-name')!.textContent =
                place.displayName!;
            infoWindowContent.children.namedItem('place-id')!.textContent =
                place.id;
            infoWindowContent.children.namedItem('place-address')!.textContent =
                place.formattedAddress!;
            infoWindow.open(map, marker);
        }
    );
}

void init();
```

> [!NOTE]
> **Note:** Read the [guide](https://developers.google.com/maps/documentation/javascript/using-typescript) on using TypeScript and Google Maps.

### JavaScript

```javascript
// This sample uses the Place Autocomplete widget to allow the user to search
// for and select a place. The sample then displays an info window containing
// the place ID and other information about the place that the user has
// selected.

async function init() {
    // Request needed libraries.
    const [{ InfoWindow }, { AdvancedMarkerElement }] = await Promise.all([
        google.maps.importLibrary('maps'),
        google.maps.importLibrary('marker'),
        google.maps.importLibrary('places'),
    ]);

    const mapElement = document.querySelector('gmp-map');
    const map = mapElement.innerMap;

    const placeAutocomplete = document.querySelector('gmp-place-autocomplete');

    // Set the map options.
    map.setOptions({
        clickableIcons: false,
        mapTypeControl: false,
        streetViewControl: false,
    });

    // Use the bounds_changed event to bias results to the current map bounds.
    map.addListener('bounds_changed', () => {
        const bounds = map.getBounds();
        if (bounds) {
            placeAutocomplete.locationBias = bounds;
        }
    });

    const infoWindow = new InfoWindow();
    const infoWindowContent = document.getElementById('infowindow-content');

    infoWindow.setContent(infoWindowContent);

    const marker = new AdvancedMarkerElement({
        map,
        collisionBehavior: 'REQUIRED_AND_HIDES_OPTIONAL',
        gmpClickable: true,
    });

    marker.addEventListener('gmp-click', () => {
        infoWindow.open(map, marker);
    });

    placeAutocomplete.addEventListener(
        'gmp-select',
        async ({ placePrediction }) => {
            infoWindow.close();

            const place = placePrediction.toPlace();

            await place.fetchFields({
                fields: ['displayName', 'formattedAddress', 'location', 'id'],
            });

            if (!place.location) {
                return;
            }

            if (place.viewport) {
                map.fitBounds(place.viewport);
            } else {
                map.setCenter(place.location);
                map.setZoom(17);
            }

            // Set the position of the marker using the place ID and location.
            marker.position = place.location;
            // marker.setVisible(true); // AdvancedMarkerElement is visible by default when map and position are set.

            infoWindowContent.children.namedItem('place-name').textContent =
                place.displayName;
            infoWindowContent.children.namedItem('place-id').textContent =
                place.id;
            infoWindowContent.children.namedItem('place-address').textContent =
                place.formattedAddress;
            infoWindow.open(map, marker);
        }
    );
}

void init();
```

### CSS

```css
/* 
 * Always set the map height explicitly to define the size of the div element
 * that contains the map. 
 */
gmp-map {
    height: 100%;
}

/* 
 * Optional: Makes the sample page fill the window. 
 */
html,
body {
    height: 100%;
    margin: 0;
    padding: 0;
    font-family: Arial, Helvetica, sans-serif;
}

#infowindow-content {
    display: none;
}

gmp-map #infowindow-content {
    display: inline;
}

gmp-place-autocomplete {
    position: absolute;
    height: 30px;
    width: 500px;
    top: 10px;
    left: 10px;
    box-shadow: 4px 4px 5px 0px rgba(0, 0, 0, 0.2);
    color-scheme: light;
    border-radius: 10px;
}
```

### HTML

```html
<html>
    <head>
        <title>Place ID Finder</title>

        <link rel="stylesheet" type="text/css" href="./style.css" />
        <script type="module" src="./index.js"></script>
        <script>
            // prettier-ignore
            (g=>{var h,a,k,p="The Google Maps JavaScript API",c="google",l="importLibrary",q="__ib__",m=document,b=window;b=b[c]||(b[c]={});var d=b.maps||(b.maps={}),r=new Set,e=new URLSearchParams,u=()=>h||(h=new Promise(async(f,n)=>{await (a=m.createElement("script"));e.set("libraries",[...r]+"");for(k in g)e.set(k.replace(/[A-Z]/g,t=>"_"+t[0].toLowerCase()),g[k]);e.set("callback",c+".maps."+q);a.src=`https://maps.${c}apis.com/maps/api/js?`+e;d[q]=f;a.onerror=()=>h=n(Error(p+" could not load."));a.nonce=m.querySelector("script[nonce]")?.nonce||"";m.head.append(a)}));d[l]?console.warn(p+" only loads once. Ignoring:",g):d[l]=(f,...n)=>r.add(f)&&u().then(()=>d[l](f,...n))})({
                key: "GOOGLE_MAPS_API_KEY"
            });
        </script>
    </head>
    <body>
        <gmp-map center="-33.8688, 151.2195" zoom="13" map-id="DEMO_MAP_ID">
            <gmp-place-autocomplete
                slot="control-inline-start-block-start"></gmp-place-autocomplete>
        </gmp-map>
        <div id="infowindow-content">
            <span id="place-name" class="title"></span><br />
            <strong>Place ID:</strong> <span id="place-id"></span><br />
            <span id="place-address"></span>
        </div>
    </body>
</html>
```

### Clone Sample


Git and Node.js are required to run this sample locally. Follow these [instructions](https://docs.npmjs.com/downloading-and-installing-node-js-and-npm)
to install Node.js and NPM. You will also need a [Google Maps API key](https://developers.google.com/maps/documentation/javascript/get-api-key).


The following commands clone the repository, install dependencies, and copy the environment file.

      git clone https://github.com/googlemaps-samples/js-api-samples.git
      cd js-api-samples
      npm i
      cp .env.template .env


Once you copy the environment file, make sure to open the `.env` file and replace `<Insert your own development API key here>` with your actual API key before starting the sample.

      cd samples/places-placeid-finder
      npm start
