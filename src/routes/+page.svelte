<script lang="ts">
	import '../app.scss';
	import Map from 'ol/Map';
	import View from 'ol/View';
	import TileLayer from 'ol/layer/Tile';
	import OSM from 'ol/source/OSM';
	import VectorLayer from 'ol/layer/Vector';
	import VectorSource from 'ol/source/Vector';
	import GeoJSON from 'ol/format/GeoJSON';
	import { Style, Fill, Stroke } from 'ol/style';
	import { onMount } from 'svelte';
	import { fromLonLat } from 'ol/proj';
	import Select from 'ol/interaction/Select';
	import { click } from 'ol/events/condition';
	import geojsonObject from './geojson.json';
	import dataImages from './dataImages.json';
	import FeatureFormat from 'ol/format/Feature';

	interface DataImage {
		url: String;
		size: String;
		date: String;
		grid: String;
		band: String;
	}

	interface PolygonProperties {
		fid: Number;
		id: Number;
		left: Number;
		top: Number;
		right: Number;
		bottom: Number;
		grid_name: String;
	}

	let map: Map;

	let featureSelectedData: PolygonProperties | null = null;

	let images: DataImage[] = [];

	onMount(() => {
		const vectorSource = new VectorSource({
			features: new GeoJSON().readFeatures(geojsonObject, {
				dataProjection: 'EPSG:4326',
				featureProjection: 'EPSG:3857' // Opcionalmente lo puedes cambiar a la proyeccion del mapa que estás utilizando
			})
		});

		const vectorLayer = new VectorLayer({
			source: vectorSource,
			style: new Style({
				fill: new Fill({
					color: 'rgba(255, 0, 0, 0.2)'
				}),
				stroke: new Stroke({
					color: 'rgba(255, 0, 0, 0.4)',
					width: 2
				})
			})
		});

		map = new Map({
			layers: [
				new TileLayer({
					source: new OSM()
				}),
				vectorLayer
			],
			target: 'map',
			view: new View({
				center: fromLonLat([-72.38936295968762, -37.07403494311157]),
				zoom: 7
			})
		});

		let selectInteraction = new Select({
			hitTolerance: 5,
			layers: [vectorLayer],
			condition: click,
			style: new Style({
				fill: new Fill({
					color: 'rgba(0, 255, 0, 0.2)'
				}),
				stroke: new Stroke({
					color: 'rgba(0, 255, 0, 0.4)',
					width: 2
				})
			})
		});

		map.addInteraction(selectInteraction);
		// Select feature when click on the reference index
		selectInteraction.on('select', function (e) {
			let f = e.selected[0];
			if (f) {
				featureSelectedData = f.getProperties() as PolygonProperties;
				if (featureSelectedData && featureSelectedData.grid_name) {
					images = dataImages.filter((d) => d.grid == featureSelectedData.grid_name);
				}
			}
		});
	});
</script>

<div id="map" />
{#if featureSelectedData && featureSelectedData.grid_name}
	<div class="info">
		<b>grid Selected: </b>
		{featureSelectedData.grid_name}

		{#each images as image}
			<p>
				{image.band}
			</p>
		{/each}
	</div>
{/if}

<style lang="scss" global>
	@import 'ol/ol.css';

	#map {
		height: 100vh;
	}
	.info {
		position: absolute;
		bottom: 2rem;
		right: 2rem;
		padding: 3rem;
		background-color: white;
		border: 1px solid grey;
		border-radius: 3px;
	}
</style>
