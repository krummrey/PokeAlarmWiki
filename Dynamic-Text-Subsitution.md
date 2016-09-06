## Overview
* [Prerequisites](#prerequities)
* [Introduction](#introduction)
* [Pokemon Text](#pokemon-text)
* [Pokestop Text](#pokestop-text)
* [Gym Text](#gym-text)
* [Reverse Location Text](#reverse-location-text)
* [Distance Matrix Text](#distance-matrix-text)
* [Example: Slack](#example-slack)

## Prerequisites
This guide assumes:

1. You have a working PokemonGo-Map installation
2. You are familiar with [JSON formatting](http://www.w3schools.com/json/default.asp)
3. You have read and understood the [Alarm Configuration](https://github.com/kvangent/PokeAlarm/wiki/Alarm-Configuration) Wiki
4. You are comfortable with the layout of `alarms.json`.

## Introduction


All alarms in the `alarms.json` alarm configuration file have customizable fields for each alert that allow you to insert your own message. This allows your to override the standard message and provide your own. You may customize as few or as many fields as you want - any field not present in your config will reset to default.

**Note:** The alarm configuration file is the only file within the PokeAlarm installation that you are **required** to directly edit (`config.ini` file is optional.)  Editing other files in your PokeAlarm installation directory is not supported.

In order to customize an Alert, you must specify what type of alert you want to config: Either `pokemon`, `pokestop`, or `gym`. Each of these has different defaults available. The following is an `alarms.json` config where a portion of the Alert has been updated:

```json
{
  "type":"slack",
  "active":"True",
  "api_key":"YOUR_API_KEY_HERE",
    "pokemon":{
        "channel":"Pokemon",
        "username":"<pkmn>",
        "title":"A GIANT <pkmn> jumped out of the grass!",
        "body": "Available until <24h_time> (<time_left>)."
    },
    "pokestop":{
        "channel":"Pokestop",
        "title":"Someone  has placed a lure on a Pokestop!",
        "body":"Better hurry! The lure only has <time_left> remaining!"
    }
}
```

The above might produce the following customized pokemon and pokestop notifications:

#### Pokemon notification output
> A GIANT Dragonite jumped out of the grass! Availble until 12:34:56 (14m 59s) remaining!

#### Pokestop notification output
> Better hurry! The lure only has 12m 34s remaining!

For what fields (title, message, etc) you have the option to change, please see the wiki page for the specific service.

## Pokemon Text


NoteThere are **two** `pokemon` keys in `alarms.json`.  The follow text substitutions below pertain to the `pokemon` field in the `alarm:[` section.

| Text           | Description                            |
|:-------------- |:---------------------------------------|
| `<id>`         | ID of the alerted pokemon              |
| `<pkmn>`       | Name of the alerted pokemon            |
| `<lat>`        | Latitude of the pokemon                |
| `<lng>`        | Longitude of the pokemon               |
| `<gmaps>`      | Gmaps link to a pin of the pokemon     |
| `<dist>`       | *Distance from set location in meters  |
| `<time_left>`  | Time remaining for the pokemon         |
| `<12h_time>`   | Dissapear time in 12hour format        |
| `<24h_time>`   | Dissapear time in 24hour format        |
| `<dir>`        | Cardinal direction from set location   |
*If no location has been set, dist will always return 0 (meters or yards)

## Pokestop Text
A list of text substitutions for the `pokestop` field in the alarm section are below:

| Text           | Description                            |
|:-------------- |:---------------------------------------|
| `<id>`         | ID of the alerted pokestop             |
| `<lat>`        | Latitude of the pokestop               |
| `<lng>`        | Longitude of the pokestop              |
| `<gmaps>`      | Gmaps link to a pin of the pokestop    |
| `<dist>`       | *Distance from set location in meters  |
| `<time_left>`  | Time remaining for the pokestop        |
| `<12h_time>`   | Expire time in 12hour format           |
| `<24h_time>`   | Expire time in 24hour format           |
| `<dir>`        | Cardinal direction from set location   |
*If no location has been set, dist will always return 0 (meters or yards)

## Gym Text
A list of text substitutions for the `gym` field in the alarm section are below:

| Text           | Description                            |
|:-------------- |:---------------------------------------|
| `<id>`         | ID of the alerted pokemon              |
| `<lat>`        | Latitude of the pokemon                |
| `<lng>`        | Longitude of the pokemon               |
| `<gmaps>`      | Gmaps link to a pin of the pokemon     |
| `<dist>`       | *Distance from set location in meters  |
| `<points>`     | Number of points the gym currently has |
| `<old_team>`   | Name of the team who lost the gym      |
| `<new_team>`   | Name of the team who captured the gym  |
*If no location has been set, dist will always return 0 (meters or yards)

## Reverse Location Text
The following text substitutions will only work if a Google Maps API Key with Geocoding API enabled has been provided to the PokeAlarm.

| Text             | Description                            |
|:---------------- |:---------------------------------------|
| `<address>`      | Address of the alert location          |
| `<postal>`       | Postal code of the alert location      |
| `<neighborhood>` | Neighborhood code of the alert location|
| `<sublocality>`  | Sublocality code of the alert location |
| `<city>`         | City code of the alert location        |
| `<county>`       | County code of the alert location      |
| `<state>`        | State code of the alert location       |
| `<county>`       | County code of the alert location      |
The fields will return `None` if unable to derive a proper location

Each pokemon, pokestop, or gym will use up 1 point of your API quota, regardless of number of fields or alarms used.

## Distance Matrix Text
The following text substitutions will only work if a Google Maps API Key with Distance Matrix API enabled has been provided to the PokeAlarm.

| Text             | Description                            |
|:---------------- |:---------------------------------------|
| `<walk_dist>`    | Estimated walking distance to the alert location |
| `<walk_time>`    | Estimated walking time to alert location|

| Text             | Description                            |
|:---------------- |:---------------------------------------|
| `<bike_dist>`    | Estimated bike distance to the alert location |
| `<bike_time>`    | Estimated bike time to alert location|


| Text             | Description		                            |
|:---------------- |:-----------------------------------------------|
| `<drive_dist>`   | Estimated drive distance to the alert location |
| `<drive_time>`   | Estimated drive time to alert location			|

Each table represents 1 API quota used for all parameters per pokemon, pokestop, or gym regardless of number of fields or alarms specified. For example, walk_time and drive_time would require 2 points, but walk_time and walk_dist would only require 1 (per alert).

## Example: Slack

Below is an example `alarm.json` for a single Slack alarm.

Let's say that a dratini spawns nearby.  You want PokeAlarm to post to your Slack **#general** channel with the format below:

>Dratini (#147) at 830 5th Avenue 10065 (328m SW) until 17:40:37 (13m 57s remaining)!

In other words, there is a dratini, pokemon number 147, at the given address, 328 meters southwest of your stated location when you launched `runwebhook.py` _(assuming you ran with the `-l` argument and provided an address or coordinates)_.  It will be there until 17:40:37 (5:40pm), and you have 13 minutes and 57 seconds to catch it.

To use this format, edit the beginning of your `alarms.json` like so, near the top where `"alarms"` is:

```
{
	"alarms":[
		{
			"active": "True",
			"type":"slack",
			"api_key":"YOUR_API_KEY",
			"startup_message":"True",
			"startup_list":"True",
			"pokemon":{
				"channel":"general",
				"username":"<pkmn>",
				"icon_url" : "https://raw.githubusercontent.com/kvangent/PokeAlarm/master/icons/<id>.png",
				"title":"<pkmn> (#<id>) at <addr> <postal>",
				"url":"<gmaps>",
				"body": "(<dist> <dir>) until <24h_time> (<time_left> remaining)!",
				"map": { 
					"enabled":"true",
					"width":"250",
					"height":"125",
					"maptype":"roadmap",
					"zoom": "15"
				}
			}
		}
	],
```

From above, we've taken advantage of the `<pkmn>`, `<id>`, `<addr>`, `<postal>`, `<gmaps>`, `<dist>`, `<dir>`, `<24h_time>`, and `<time_left>` variables to customize our notification message of the nearby dratini.

* Add your own [Slack API Key](https://get.slack.help/hc/en-us/articles/215770388-Creating-and-regenerating-API-tokens) after `"api_key"`.
* Change "general" in `"Channel":"#general"` to whatever channel you'd like PokeAlarm to post to.
* Edit the` "title"` key to whatever you'd like, using the above text substitutions to customize your messages.
* The `"url"` key will hyperlink your title to google maps directions.
* The `"body"` key is additional text in slack that will not be hyperlinked.
* Change the `"width"`, `"height"`, `"maptype"`, and `"zoom"` of the static map as you'd like
* Be conscious about JSON format

Great.  Now that you have your formatting perfect, you want to handpick your pokemon to be notified of.  You can see in the pokemon list below that some are set to `"True"` (Lapras, Kabutops) while others are set to a numeric value, e.g. `"75"` (eevee) or `"1000"` (dragonite, etc.) The numeric values indicate to PokeAlarm that you want to be notified only if that pokemon is within x meters of a given location (again, assuming `python ./runwebhook.py -l 'your location'`).  If you don't supply a location then any of the pokemon in the list with numeric values default to `"True"`, and you'll get notified of that Eevee and Dragonite, no matter how far away.


Below is a complete `alarms.json` for Slack:

```
{
	"alarms":[
		{
			"active": "True",
			"type":"slack",
			"api_key":"YOUR_API_KEY",
			"startup_message":"True",
			"startup_list":"True",
			"pokemon":{
				"channel":"general",
				"username":"<pkmn>",
				"icon_url" : "https://raw.githubusercontent.com/kvangent/PokeAlarm/master/icons/<id>.png",
				"title":"A wild <pkmn> has appeared!",
				"url":"<gmaps>",
				"body": "Available until <24h_time> (<time_left>).",
				"map": { 
					"enabled":"true",
					"width":"250",
					"height":"125",
					"maptype":"roadmap",
					"zoom": "15"
				}
			},
			"pokestop":{
				"channel":"general",
				"username":"Pokestop",
				"icon_url" : "https://raw.githubusercontent.com/kvangent/PokeAlarm/master/icons/pokestop.png",
				"title":"Someone has placed a lure on a Pokestop!",
				"url":"<gmaps>",
				"body":"Lure will expire at <24h_time> (<time_left>)."
			},
			"gym":{
				"channel":"general",
				"username":"Pokemon Gym",
				"icon_url" : "https://raw.githubusercontent.com/kvangent/PokeAlarm/master/icons/gym.png",
				"title":"A Team <old_team> gym has fallen!",
				"url":"<gmaps>",
				"body": "It is now controlled by <new_team>."
			}
		}
	],
	"pokemon":{
		"Bulbasaur":"75",
		"Ivysaur":"75",
		"Venusaur":"1000",
		"Charmander":"75",
		"Charmeleon":"75",
		"Charizard":"1000",
		"Squirtle":"75",
		"Wartortle":"75",
		"Blastoise":"1000",
		"Caterpie":"False",
		"Metapod":"False",
		"Butterfree":"False",
		"Weedle":"False",
		"Kakuna":"False",
		"Beedrill":"False",
		"Pidgey":"False",
		"Pidgeotto":"False",
		"Pidgeot":"False",
		"Rattata":"False",
		"Raticate":"False",
		"Spearow":"False",
		"Fearow":"False",
		"Ekans":"False",
		"Arbok":"75",
		"Pikachu":"75",
		"Raichu":"100",
		"Sandshrew":"False",
		"Sandslash":"75",
		"Nidoran♀":"False",
		"Nidorina":"False",
		"Nidoqueen":"75",
		"Nidoran♂":"False",
		"Nidorino":"False",
		"Nidoking":"75",
		"Clefairy":"False",
		"Clefable":"75",
		"Vulpix":"75",
		"Ninetales":"75",
		"Jigglypuff":"False",
		"Wigglytuff":"75",
		"Zubat":"False",
		"Golbat":"False",
		"Oddish":"False",
		"Gloom":"75",
		"Vileplume":"75",
		"Paras":"False",
		"Parasect":"75",
		"Venonat":"False",
		"Venomoth":"75",
		"Diglett":"False",
		"Dugtrio":"75",
		"Meowth":"False",
		"Persian":"75",
		"Psyduck":"False",
		"Golduck":"75",
		"Mankey":"False",
		"Primeape":"False",
		"Growlithe":"False",
		"Arcanine":"1000",
		"Poliwag":"False",
		"Poliwhirl":"75",
		"Poliwrath":"1000",
		"Abra":"False",
		"Kadabra":"False",
		"Alakazam":"1000",
		"Machop":"False",
		"Machoke":"75",
		"Machamp":"1000",
		"Bellsprout":"False",
		"Weepinbell":"75",
		"Victreebel":"75",
		"Tentacool":"False",
		"Tentacruel":"75",
		"Geodude":"False",
		"Graveler":"75",
		"Golem":"75",
		"Ponyta":"75",
		"Rapidash":"75",
		"Slowpoke":"75",
		"Slowbro":"False",
		"Magnemite":"False",
		"Magneton":"75",
		"Farfetch'd":"False",
		"Doduo":"False",
		"Dodrio":"75",
		"Seel":"False",
		"Dewgong":"False",
		"Grimer":"False",
		"Muk":"False",
		"Shellder":"False",
		"Cloyster":"75",
		"Gastly":"False",
		"Haunter":"75",
		"Gengar":"75",
		"Onix":"75",
		"Drowzee":"False",
		"Hypno":"False",
		"Krabby":"False",
		"Kingler":"75",
		"Voltorb":"False",
		"Electrode":"False",
		"Exeggcute":"False",
		"Exeggutor":"75",
		"Cubone":"False",
		"Marowak":"False",
		"Hitmonlee":"False",
		"Hitmonchan":"False",
		"Lickitung":"False",
		"Koffing":"False",
		"Weezing":"False",
		"Rhyhorn":"False",
		"Rhydon":"False",
		"Chansey":"1000",
		"Tangela":"False",
		"Kangaskhan":"False",
		"Horsea":"False",
		"Seadra":"False",
		"Goldeen":"False",
		"Seaking":"75",
		"Staryu":"False",
		"Starmie":"75",
		"Mr. Mime":"False",
		"Scyther":"False",
		"Jynx":"False",
		"Electabuzz":"False",
		"Magmar":"False",
		"Pinsir":"False",
		"Tauros":"False",
		"Magikarp":"False",
		"Gyarados":"1000",
		"Lapras":"True",
		"Ditto":"False",
		"Eevee":"75",
		"Vaporeon":"1000",
		"Jolteon":"1000",
		"Flareon":"1000",
		"Porygon":"1000",
		"Omanyte":"1000",
		"Omastar":"1000",
		"Kabuto":"1000",
		"Kabutops":"True",
		"Aerodactyl":"1000",
		"Snorlax":"1000",
		"Articuno":"1000",
		"Zapdos":"1000",
		"Moltres":"1000",
		"Dratini":"1000",
		"Dragonair":"1000",
		"Dragonite":"1000",
		"Mewtwo":"1000",
		"Mew":"1000"
	}
}
```