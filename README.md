# `covid`

## What

A long-winded Bash script for querying the [Coronavirus Tracker API](https://github.com/ExpDev07/coronavirus-tracker-api).
By default, `covid` loads the JSON files to disk (`$XDG_CACHE_HOME` or `$HOME/.cache`) and queries them locally. If you don't want this behavior, run the script with the `-n, --no-cache` option.

## Dependencies

[jq](https://github.com/stedolan/jq)

## How

```text
$ covid <country> [, [state|province], [county]]
```
All params are case-insensitive. 

* `<country>`: Either an ISO 3166 country code (`US`, `ca`, `Au`, ...) or `world` if you want to view the worldwide stats.
* `[state|province]`: Full name of the state or province (`alberta`, `California`, ...). Only available for some countries.
	* `[county]`: Full name of the county (`los angeles`, `new york`, `orange` ...) without the 'county' part. Only available for U.S. state queries.

### Examples:

```text
$ covid us
US

Confirmed: 101,657 (+17,821)
   Deaths:   1,581 (+372)

latest snapshot 2020-03-27
     delta from 2020-03-26
     API reload 2020-03-28 17:55
```

```text
$ covid ca, ontario
Canada >> Ontario

Confirmed: 994 (+136)
   Deaths:  18 (+5)

latest snapshot 2020-03-27
     delta from 2020-03-26
     API reload 2020-03-28 17:55
```

```text
$ covid us, california, los angeles
US >> California > Los Angeles County

Confirmed: 1,465
   Deaths:    26

     API reload 2020-03-28 13:52
```

### Other options

* `-c, --csv`: Output as comma-separated values
	* Columns: `country code, province, county, confirmed, deaths, recovered, Δ confirmed, Δ deaths, Δ recovered, latest snapshot datetime`
	* Example:
```text
$ covid us, new york, new york -c
us,new york,new york,29158,517,0,,,,
```

* `-f, --force-refresh`: Forcibly reload cached JSON files. Normally, `covid` reloads cached files if they're more than 6 hours old.

* `-n, --no-cache`: Run the query directly against the API. Very wasteful.

* `-h, --help`: Help.

## Caveats

* The API doesn't track the number of people recovered from the virus for the time being. You'll have to get that from somewhere else in the meantime.

* These datapoints are midnight snapshots, not hourly updates. 

* U.S. queries for state-level or county-level data lack the `timelines` object. They'll be displayed without the corresponding previous-snapshot delta values.
