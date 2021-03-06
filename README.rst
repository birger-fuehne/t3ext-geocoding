TYPO3 Extension: Geocoding
======================================

Provides services for querying Google Maps GeoCoding API v3 in your own extensions.

* Extension Key: geocoding
* Author: Benjamin Mack, b:dreizehn GmbH, 2012-2013
* Licensed under: GPL 2+
* Required TYPO3 6.2+
* All code can be found and developed on github: 

Introduction
------------
This extension provides an abstract way to get geo coordinates of addresses around the world. "Geocoding" let's you fetch information about an address and stores it in the DB, by using the TYPO3 Caching Framework to store the queries and results.

Configuration
-------------
Fetch a google API key (https://code.google.com/apis/console) and add it to the extension configuration info in the Extension Manager. For more information see here: https://developers.google.com/maps/documentation/geocoding/?hl=en

How to use
----------
Instantiate the class via GeneralUtility::makeInstance() or the object manager in your TYPO3 extension. Then use the public methods.

GeoService
----------
The extension provides you with a clean PHP/TYPO3 extension abstraction to fetch latitude and longitude for a specific address string. This is done in the GeoService.php Service PHP class.

.. GeoService->calculateCoordinatesForAllRecordsInTable

If you need to query user input, a JavaScript API is probably the best way to do so. However, it can be done via JS as well, by calling GeoService->getCoordinatesForAddress($street = NULL, $zip = NULL, $city = NULL, $country = 'Germany')

	$geoServiceObject = GeneralUtility::makeInstance(\B13\Geocoding\Service\GeoService::class);
	$coordinates = $geoServiceObject->getCoordinatesForAddress('Breitscheidstr. 65', 70178, 'Stuttgart', 'Germany');

The method does internal caching of the same requests.

.. GeoService->calculateCoordinatesForAllRecordsInTable

The method "GeoService->calculateCoordinatesForAllRecordsInTable($tableName, $latitudeField, $longitudeField, $streetField, $zipField, $cityField, $countryField, $addWhereClause)" allows you to bulk-encode latitude and longitude fields for existing addresses. The call can easily be built inside a Scheduler Task.

This you can fetch the information about an address of a DB record (e.g. tt_address) and store the data in the database table, given that you add two new fields latitude and longitute to that target table in your extension (no TCA information required for that).


RadiusService
-------------
The other main service class is used for the calculating distances between two coordinates (RadiusService->getDistance(), and querying records from a DB table with latitude and longitude (works perfectly in conjunction with calculateCoordinatesForAllRecordsInTable()) given a certain radius and base coordinates.


Thanks / Contributions
----------------------

Thanks go to

* The crew at b13, making use of these features
* Jesus Christ, who saved my life.

2013-07-05, Benni.
