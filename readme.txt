=== Nearby Map by Wabeo ===
Contributors: willybahuaud
Tags: leaflet, around map, geolocalization, route, places, cloudmade, openstreetmap, events organization
Requires at least: 3.0
Tested up to: 3.5.1
Stable tag: 3.5.1
License: GPLv2 or later
License URI: http://www.gnu.org/licenses/gpl-2.0.html

Allow to build a map to show the activities, places and services around a given geographical point.

== Description ==

Allow to build a map to show the activities, places and services around a given geographical point.

This plugin use leaflet, OpenStreetMap and ClouMade, instead of Google Map.

== Installation ==

1. Upload the plugin's folder into `/wp-content/plugins/` directory
2. Activate the plugin through the 'Plugins' menu in WordPress
3. Use `[maps]` into your content area, or use `<?php echo nbm_render_map(); ?>` into one template file to render the map
4. Use `[place]` into your place content area, or use `<?php echo nbm_place_information(); ?>` into your single place (or specified CPT) template file to show information about current place
5. For working correctly, you need to enter at least 1 place, and you also need to define 1 of places as the central place

== Frequently Asked Questions ==

= How can I use an existing post type, instead of let the plugin creating one ? =

You just have to modify then paste this following code into your functions.php theme file.
`<?php add_filter( 'nbm_post_type', 'function_for_alter' );
function function_for_alter(){
	return 'posts';
} ?>`

= Is there a way to use another tile provider than CloudMade ? =

Yes, there are other tile provider than CloudMade (used by default in this plugin). To chose for another, simply paste this function into your functions.php.
`<?php add_filter( 'maps_datas', 'function_for_alter' );
function function_for_alter( $maps_datas ){
	$maps_datas['tiles'] = "http://{s}.mqcdn.com/tiles/1.0.0/map/{z}/{x}/{y}.jpg";
	$maps_datas['attribution'] = "attribution I want/use to show";
	$maps_datas['subdomains'] = array('otile1','otile2','otile3','otile4');
	return $maps_datas;
} ?>`

I tested some tiles providers, and I confirm they work with Nearby Map :

* mapquest (this one need to precise subdomains) :
 * http://otile1.mqcdn.com/tiles/1.0.0/map/{z}/{x}/{y}.jpg
 * http://otile1.mqcdn.com/tiles/1.0.0/sat/{z}/{x}/{y}.jpg 
   * this one dont deliver tiles for high zoom level (except for USA)
* openstreetmap :
 * http://{s}.tile.openstreetmap.org/{z}/{x}/{y}.png
 * http://{s}.tile.osm.org/{z}/{x}/{y}.png
* mapbox
 * http://{s}.tiles.mapbox.com/v3/{user}.{map}/{z}/{x}/{y}.png
* OpenCycleMap
 * http://{s}.tile.opencyclemap.org/cycle/{z}/{x}/{y}.png
* Stamen
 * http://{s}.tile.stamen.com/toner/{z}/{x}/{y}.png
* ESRI
 * http://server.arcgisonline.com/ArcGIS/rest/services/World_Street_Map/MapServer/tile/{z}/{y}/{x}.png
 * http://server.arcgisonline.com/ArcGIS/rest/services/World_Shaded_Relief/MapServer/tile/{z}/{y}/{x}.png
* Open Weather Map
 * http://{s}.tile.openweathermap.org/map/clouds/{z}/{x}/{y}.png

= Can I change default post type properties ? =

Yes, doing this into your functions.php theme file.
`<?php add_filter( 'places_args', 'function_for_alter' );
function function_for_alter( $args ){
	$args['rewrite'] = array( 'slug', 'local business' );
	$args['supports'][] = 'custom-fields';
	return $args;
} ?>`

= Locate places with Nearby Map seem to be imprecise, can I improve precision of returned coordinates ? =

Just paste this :
`<?php add_filter( 'nbm_try_to_find_with_openstreetmap', '__return_false' ); ?>`

= I already have a CloudMade API key, can I use it ? =
`<?php add_filter( 'cloudmate_key', 'function_for_alter' );
function function_for_alter(){
	return 'dfsljfdjfsdjfqsjdkdfjkfqf'; //for example
} ?>`

= I dont want to see a list of all place behind the map, how can I remove it ? =
`<?php add_filter( 'nbm_need_more', '__return_false' ); ?>`

= I dont need route system also, how can I remove it ? =
`<?php add_filter( 'nbm_need_route', '__return_false' ); ?>`

= I dont have single page for Place, so I want to remove the link. Can I do ? =
`<?php add_filter( 'nbm_places_link', '__return_false' ); ?>`

= I want to proceed some change on the place query =
`<?php add_filter( 'markers_querys', 'function_for_alter' );
function function_for_alter( $m ){
	$m['order'] = 'DESC';
	$m['tax_query'] = array(
		array(
			'taxonomy' => 'type_of_place',
			'field' => 'id',
			'term' => 56
		)
	);
	return $m;
} ?>`

= I want to alter something in HTML returned for all places =
`<?php add_filter( 'nbm_map', 'function_for_alter' );
function function_for_alter(){
	//Do stuff...
} ?>`

= I want to alter something in HTML returned datas of single place information =
`<?php add_filter( 'nbm_place_information', 'function_for_alter' );
function function_for_alter(){
	//Do stuff...
} ?>`

== Screenshots ==

1. View of the main map, during a route animation

== Changelog ==

= 0.9 =
* Initial release
