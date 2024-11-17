<script>
	import Map from 'ol/Map';
	import View from 'ol/View';
	import TileLayer from 'ol/layer/Tile';
	import VectorLayer from 'ol/layer/Vector';
	import VectorSource from 'ol/source/Vector';
	import OSM from 'ol/source/OSM';
	import Feature from 'ol/Feature';
	import Point from 'ol/geom/Point';
	import { fromLonLat, get as getProjection } from 'ol/proj';
	import Style from 'ol/style/Style';
	import CircleStyle from 'ol/style/Circle';
	import Fill from 'ol/style/Fill';
	import Stroke from 'ol/style/Stroke';
	import { extend } from 'ol/extent';
	import { sharedState } from '../shared/store';

	 let {data} = $props()

	let currentValue = null;
	let map;
	let vectorSource;


	sharedState.subscribe((value) => {
		if(value === "null"){
			currentValue = null
		}else{
			currentValue = value;
		}
		filterData();
	});


	function filterData() {

		const filteredData = currentValue
			? data.filter((dt) => dt.status.toLowerCase() === currentValue.toLowerCase())
			: data;

	
		updateMap(filteredData);
	}


	function updateMap(filteredData) {
		if (!map || !vectorSource) return;
		vectorSource.clear();
		const features = [];
		let extent = undefined;

		filteredData.forEach((location) => {
			const { latitude, longitude, name } = location.location;

			const feature = new Feature({
				geometry: new Point(fromLonLat([longitude, latitude])),
				name: name
			});

			feature.setStyle(
				new Style({
					image: new CircleStyle({
						radius: 10,
						fill: new Fill({ color: 'rgba(0, 255, 0, 0.8)' }),
						stroke: new Stroke({ color: '#00FF00', width: 2 })
					})
				})
			);

			features.push(feature);

			const featureExtent = feature.getGeometry().getExtent();
			extent = extent ? extend(extent, featureExtent) : featureExtent;
		});


		vectorSource.addFeatures(features);


		if (extent) {
			map.getView().fit(extent, { padding: [50, 50, 50, 50], duration: 1000 });
		}
	}

	$effect(() => {
		vectorSource = new VectorSource();

		const vectorLayer = new VectorLayer({
			source: vectorSource
		});

		map = new Map({
			target: 'map',
			layers: [
				new TileLayer({
					source: new OSM()
				}),
				vectorLayer
			],
			view: new View({
				projection: getProjection('EPSG:3857'),
				zoom: 7
			})
		});

		updateMap(data);
	});
</script>

<div id="map"></div>

<style>
	#map {
		height: 35vh;
		border: 2px solid #ccc;
		border-radius: 8px;
		box-shadow: 0px 4px 10px rgba(0, 0, 0, 0.1);
	}

	body {
		margin: 0;
		font-family: Arial, sans-serif;
		background: #f5f5f5;
	}
</style>
