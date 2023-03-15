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
	import type Feature from 'ol/Feature';

	let map: Map;

	let featureSelectedData = {};

	onMount(() => {
		const geojsonObject = {
			type: 'FeatureCollection',
			name: 'grilla_incendios_utm19_v2',
			features: [
				{
					type: 'Feature',
					properties: {
						fid: 1,
						id: 3,
						left: 50000.0,
						top: 6050000.0,
						right: 150000.0,
						bottom: 5950000.0,
						grid_name: '5-15_605-595'
					},
					geometry: {
						type: 'Polygon',
						coordinates: [
							[
								[-73.9654058, -35.5914409],
								[-74.0222624, -36.4895972],
								[-72.908888, -36.5313609],
								[-72.8645478, -35.6318583],
								[-73.9654058, -35.5914409]
							]
						]
					}
				},
				{
					type: 'Feature',
					properties: {
						fid: 2,
						id: 4,
						left: 50000.0,
						top: 5950000.0,
						right: 150000.0,
						bottom: 5850000.0,
						grid_name: '5-15_595-585'
					},
					geometry: {
						type: 'Polygon',
						coordinates: [
							[
								[-74.0222624, -36.4895972],
								[-74.0816932, -37.3875382],
								[-72.9552391, -37.4306792],
								[-72.908888, -36.5313609],
								[-74.0222624, -36.4895972]
							]
						]
					}
				},
				{
					type: 'Feature',
					properties: {
						fid: 3,
						id: 5,
						left: 50000.0,
						top: 5850000.0,
						right: 150000.0,
						bottom: 5750000.0,
						grid_name: '5-15_585-575'
					},
					geometry: {
						type: 'Polygon',
						coordinates: [
							[
								[-74.0816932, -37.3875382],
								[-74.1438197, -38.285258],
								[-73.0036961, -38.3298089],
								[-72.9552391, -37.4306792],
								[-74.0816932, -37.3875382]
							]
						]
					}
				},
				{
					type: 'Feature',
					properties: {
						fid: 4,
						id: 6,
						left: 50000.0,
						top: 5750000.0,
						right: 150000.0,
						bottom: 5650000.0,
						grid_name: '5-15_575-565'
					},
					geometry: {
						type: 'Polygon',
						coordinates: [
							[
								[-74.1438197, -38.285258],
								[-74.2087719, -39.1827508],
								[-73.0543612, -39.2287462],
								[-73.0036961, -38.3298089],
								[-74.1438197, -38.285258]
							]
						]
					}
				},
				{
					type: 'Feature',
					properties: {
						fid: 5,
						id: 7,
						left: 50000.0,
						top: 5650000.0,
						right: 150000.0,
						bottom: 5550000.0,
						grid_name: '5-15_565-555'
					},
					geometry: {
						type: 'Polygon',
						coordinates: [
							[
								[-74.2087719, -39.1827508],
								[-74.27669, -40.0800104],
								[-73.1073441, -40.127487],
								[-73.0543612, -39.2287462],
								[-74.2087719, -39.1827508]
							]
						]
					}
				},
				{
					type: 'Feature',
					properties: {
						fid: 6,
						id: 9,
						left: 150000.0,
						top: 6250000.0,
						right: 250000.0,
						bottom: 6150000.0,
						grid_name: '15-25_625-615'
					},
					geometry: {
						type: 'Polygon',
						coordinates: [
							[
								[-72.7815509, -33.8323155],
								[-72.8221296, -34.7321752],
								[-71.7314105, -34.7615543],
								[-71.7023702, -33.860725],
								[-72.7815509, -33.8323155]
							]
						]
					}
				},
				{
					type: 'Feature',
					properties: {
						fid: 7,
						id: 10,
						left: 150000.0,
						top: 6150000.0,
						right: 250000.0,
						bottom: 6050000.0,
						grid_name: '15-25_615-605'
					},
					geometry: {
						type: 'Polygon',
						coordinates: [
							[
								[-72.8221296, -34.7321752],
								[-72.8645478, -35.6318583],
								[-71.7617686, -35.662228],
								[-71.7314105, -34.7615543],
								[-72.8221296, -34.7321752]
							]
						]
					}
				},
				{
					type: 'Feature',
					properties: {
						fid: 8,
						id: 11,
						left: 150000.0,
						top: 6050000.0,
						right: 250000.0,
						bottom: 5950000.0,
						grid_name: '15-25_605-595'
					},
					geometry: {
						type: 'Polygon',
						coordinates: [
							[
								[-72.8645478, -35.6318583],
								[-72.908888, -36.5313609],
								[-71.793504, -36.5627432],
								[-71.7617686, -35.662228],
								[-72.8645478, -35.6318583]
							]
						]
					}
				},
				{
					type: 'Feature',
					properties: {
						fid: 9,
						id: 12,
						left: 150000.0,
						top: 5950000.0,
						right: 250000.0,
						bottom: 5850000.0,
						grid_name: '15-25_595-585'
					},
					geometry: {
						type: 'Polygon',
						coordinates: [
							[
								[-72.908888, -36.5313609],
								[-72.9552391, -37.4306792],
								[-71.8266803, -37.4630973],
								[-71.793504, -36.5627432],
								[-72.908888, -36.5313609]
							]
						]
					}
				},
				{
					type: 'Feature',
					properties: {
						fid: 10,
						id: 13,
						left: 150000.0,
						top: 5850000.0,
						right: 250000.0,
						bottom: 5750000.0,
						grid_name: '15-25_585-575'
					},
					geometry: {
						type: 'Polygon',
						coordinates: [
							[
								[-72.9552391, -37.4306792],
								[-73.0036961, -38.3298089],
								[-71.8613658, -38.3632877],
								[-71.8266803, -37.4630973],
								[-72.9552391, -37.4306792]
							]
						]
					}
				},
				{
					type: 'Feature',
					properties: {
						fid: 11,
						id: 14,
						left: 150000.0,
						top: 5750000.0,
						right: 250000.0,
						bottom: 5650000.0,
						grid_name: '15-25_575-565'
					},
					geometry: {
						type: 'Polygon',
						coordinates: [
							[
								[-73.0036961, -38.3298089],
								[-73.0543612, -39.2287462],
								[-71.8976341, -39.2633117],
								[-71.8613658, -38.3632877],
								[-73.0036961, -38.3298089]
							]
						]
					}
				},
				{
					type: 'Feature',
					properties: {
						fid: 12,
						id: 15,
						left: 150000.0,
						top: 5650000.0,
						right: 250000.0,
						bottom: 5550000.0,
						grid_name: '15-25_565-555'
					},
					geometry: {
						type: 'Polygon',
						coordinates: [
							[
								[-73.0543612, -39.2287462],
								[-73.1073441, -40.127487],
								[-71.9355638, -40.1631668],
								[-71.8976341, -39.2633117],
								[-73.0543612, -39.2287462]
							]
						]
					}
				},
				{
					type: 'Feature',
					properties: {
						fid: 13,
						id: 17,
						left: 250000.0,
						top: 6250000.0,
						right: 350000.0,
						bottom: 6150000.0,
						grid_name: '25-35_625-615'
					},
					geometry: {
						type: 'Polygon',
						coordinates: [
							[
								[-71.7023702, -33.860725],
								[-71.7314105, -34.7615543],
								[-70.6393743, -34.7811662],
								[-70.6219279, -33.8796892],
								[-71.7023702, -33.860725]
							]
						]
					}
				},
				{
					type: 'Feature',
					properties: {
						fid: 14,
						id: 18,
						left: 250000.0,
						top: 6150000.0,
						right: 350000.0,
						bottom: 6050000.0,
						grid_name: '25-35_615-605'
					},
					geometry: {
						type: 'Polygon',
						coordinates: [
							[
								[-71.7314105, -34.7615543],
								[-71.7617686, -35.662228],
								[-70.657613, -35.6825015],
								[-70.6393743, -34.7811662],
								[-71.7314105, -34.7615543]
							]
						]
					}
				},
				{
					type: 'Feature',
					properties: {
						fid: 15,
						id: 19,
						left: 250000.0,
						top: 6050000.0,
						right: 350000.0,
						bottom: 5950000.0,
						grid_name: '25-35_605-595'
					},
					geometry: {
						type: 'Polygon',
						coordinates: [
							[
								[-71.7617686, -35.662228],
								[-71.793504, -36.5627432],
								[-70.6766798, -36.5836931],
								[-70.657613, -35.6825015],
								[-71.7617686, -35.662228]
							]
						]
					}
				},
				{
					type: 'Feature',
					properties: {
						fid: 16,
						id: 20,
						left: 250000.0,
						top: 5950000.0,
						right: 350000.0,
						bottom: 5850000.0,
						grid_name: '25-35_595-585'
					},
					geometry: {
						type: 'Polygon',
						coordinates: [
							[
								[-71.793504, -36.5627432],
								[-71.8266803, -37.4630973],
								[-70.696613, -37.4847392],
								[-70.6766798, -36.5836931],
								[-71.793504, -36.5627432]
							]
						]
					}
				},
				{
					type: 'Feature',
					properties: {
						fid: 17,
						id: 21,
						left: 250000.0,
						top: 5850000.0,
						right: 350000.0,
						bottom: 5750000.0,
						grid_name: '25-35_585-575'
					},
					geometry: {
						type: 'Polygon',
						coordinates: [
							[
								[-71.8266803, -37.4630973],
								[-71.8613658, -38.3632877],
								[-70.7174537, -38.3856381],
								[-70.696613, -37.4847392],
								[-71.8266803, -37.4630973]
							]
						]
					}
				},
				{
					type: 'Feature',
					properties: {
						fid: 18,
						id: 22,
						left: 250000.0,
						top: 5750000.0,
						right: 350000.0,
						bottom: 5650000.0,
						grid_name: '25-35_575-565'
					},
					geometry: {
						type: 'Polygon',
						coordinates: [
							[
								[-71.8613658, -38.3632877],
								[-71.8976341, -39.2633117],
								[-70.7392463, -39.2863882],
								[-70.7174537, -38.3856381],
								[-71.8613658, -38.3632877]
							]
						]
					}
				},
				{
					type: 'Feature',
					properties: {
						fid: 19,
						id: 23,
						left: 250000.0,
						top: 5650000.0,
						right: 350000.0,
						bottom: 5550000.0,
						grid_name: '25-35_565-555'
					},
					geometry: {
						type: 'Polygon',
						coordinates: [
							[
								[-71.8976341, -39.2633117],
								[-71.9355638, -40.1631668],
								[-70.7620383, -40.186988],
								[-70.7392463, -39.2863882],
								[-71.8976341, -39.2633117]
							]
						]
					}
				}
			]
		};

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

		var selectInteraction = new Select({
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
			var f = e.selected[0];
			if (f) {
				featureSelectedData = f.getProperties();
			}
		});
	});
</script>

<div id="map" />
{#if featureSelectedData && featureSelectedData.grid_name}
	<div class="info">
		<b>grid Selected: </b>
		{featureSelectedData.grid_name}
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