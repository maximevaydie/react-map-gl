[Sources](https://docs.mapbox.com/mapbox-gl-js/api/#sources) specify the geographic features to be rendered on the map.

## GeoJSON Source

A [GeoJSON source](https://docs.mapbox.com/mapbox-gl-js/style-spec/#sources-geojson). Data must be provided via a `data` property, whose value can be a URL or inline GeoJSON.

```jsx
import React from 'react';
import MapGL, { Source, Layer } from '@urbica/react-map-gl';
import 'mapbox-gl/dist/mapbox-gl.css';

initialState = {
  viewport: {
    latitude: 45.137451890638886,
    longitude: -68.13734351262877,
    zoom: 5
  }
};

const data = {
  type: 'Feature',
  geometry: {
    type: 'Polygon',
    coordinates: [
      [
        [-67.13734351262877, 45.137451890638886],
        [-66.96466, 44.8097],
        [-68.03252, 44.3252],
        [-69.06, 43.98],
        [-70.11617, 43.68405],
        [-70.64573401557249, 43.090083319667144],
        [-70.75102474636725, 43.08003225358635],
        [-70.79761105007827, 43.21973948828747],
        [-70.98176001655037, 43.36789581966826],
        [-70.94416541205806, 43.46633942318431],
        [-71.08482, 45.3052400000002],
        [-70.6600225491012, 45.46022288673396],
        [-70.30495378282376, 45.914794623389355],
        [-70.00014034695016, 46.69317088478567],
        [-69.23708614772835, 47.44777598732787],
        [-68.90478084987546, 47.184794623394396],
        [-68.23430497910454, 47.35462921812177],
        [-67.79035274928509, 47.066248887716995],
        [-67.79141211614706, 45.702585354182816],
        [-67.13734351262877, 45.137451890638886]
      ]
    ]
  }
};

<MapGL
  style={{ width: '100%', height: '400px' }}
  mapStyle='mapbox://styles/mapbox/light-v9'
  accessToken={MAPBOX_ACCESS_TOKEN}
  onViewportChange={viewport => setState({ viewport })}
  {...state.viewport}
>
  <Source id='maine' type='geojson' data={data} />
  <Layer
    id='maine'
    type='fill'
    source='maine'
    paint={{
      'fill-color': '#088',
      'fill-opacity': 0.8
    }}
  />
</MapGL>;
```

### Updating GeoJSON Source Data

```jsx
import React from 'react';
import { randomPoint } from '@turf/random';
import MapGL, { Source, Layer } from '@urbica/react-map-gl';
import 'mapbox-gl/dist/mapbox-gl.css';

initialState = {
  points: randomPoint(100),
  viewport: {
    latitude: 0,
    longitude: 0,
    zoom: 0
  }
};

const addPoints = () => {
  const randomPoints = randomPoint(100);
  const newFeatures = state.points.features.concat(randomPoints.features);
  const newPoints = { ...state.points, features: newFeatures };
  setState({ points: newPoints });
};

<React.Fragment>
  <button onClick={addPoints}>+100 points</button>
  <MapGL
    style={{ width: '100%', height: '400px' }}
    mapStyle='mapbox://styles/mapbox/light-v9'
    accessToken={MAPBOX_ACCESS_TOKEN}
    onViewportChange={viewport => setState({ viewport })}
    {...state.viewport}
  >
    <Source id='points' type='geojson' data={state.points} />
    <Layer
      id='points'
      type='circle'
      source='points'
      paint={{
        'circle-radius': 6,
        'circle-color': '#1978c8'
      }}
    />
  </MapGL>
</React.Fragment>;
```

## Vector Source

Add a [vector source](https://docs.mapbox.com/mapbox-gl-js/style-spec/#sources-vector) to a map.

```jsx
import React from 'react';
import MapGL, { Source, Layer } from '@urbica/react-map-gl';
import 'mapbox-gl/dist/mapbox-gl.css';

initialState = {
  viewport: {
    latitude: 37.753574,
    longitude: -122.447303,
    zoom: 13
  }
};

<MapGL
  style={{ width: '100%', height: '400px' }}
  mapStyle='mapbox://styles/mapbox/light-v9'
  accessToken={MAPBOX_ACCESS_TOKEN}
  onViewportChange={viewport => setState({ viewport })}
  {...state.viewport}
>
  <Source id='contour' type='vector' url='mapbox://mapbox.mapbox-terrain-v2' />
  <Layer
    id='contour'
    type='line'
    source='contour'
    source-layer='contour'
    paint={{
      'line-color': '#877b59',
      'line-width': 1
    }}
  />
</MapGL>;
```

## Raster Source

```jsx
import React from 'react';
import MapGL, { Source, Layer } from '@urbica/react-map-gl';
import 'mapbox-gl/dist/mapbox-gl.css';

initialState = {
  viewport: {
    latitude: 40.6892,
    longitude: -74.5447,
    zoom: 8
  }
};

<MapGL
  style={{ width: '100%', height: '400px' }}
  mapStyle='mapbox://styles/mapbox/light-v9'
  accessToken={MAPBOX_ACCESS_TOKEN}
  onViewportChange={viewport => setState({ viewport })}
  {...state.viewport}
>
  <Source
    id='wms-test-layer'
    type='raster'
    tileSize={256}
    tiles={[
      'https://geodata.state.nj.us/imagerywms/Natural2015?bbox={bbox-epsg-3857}&format=image/png&service=WMS&version=1.1.1&request=GetMap&srs=EPSG:3857&transparent=true&width=256&height=256&layers=Natural2015'
    ]}
  />
  <Layer
    id='wms-test-layer'
    type='raster'
    source='wms-test-layer'
    before='aeroway-taxiway'
  />
</MapGL>;
```