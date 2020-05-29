---
description: >-
  A service that traverses Granary's APIs to reveal lineage information between
  Harvester Instances, Event Types, Belts, Profile Types, and Segments.
---

# Lineage Report

## Description

Uses Granary's APIs to export information from Profile Store as well as Event Store to generate lineage reports. It exports them to two separated CSV files defined in the configuration file , eventStoreExport.csv\). In the default mode, the service selects one Correlation ID per profile type to generate the report. By defining one or more profileTypes and/or correlationIds in the config file this behaviour could be changed to export only the specified combinations of profileTypes and correlationId's.

## Usage

The code can be obtained from the [Granary Lineage Report Service repository](https://gitlab.alvary.io/grnry/grnry-lineage-report). For service execution, a Python 3.7 distribution and access to Granary APIs are required. To start the service, run

```text
    ./gen_lineage_report.py
```

## Configuration

Configuration file is ./config.py

### Parameters

<table>
  <thead>
    <tr>
      <th style="text-align:left">Property</th>
      <th style="text-align:left">Description</th>
      <th style="text-align:left">Default</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="text-align:left">
        <p><b>credentials</b>
        </p>
        <ul>
          <li><b>grant_type</b> 
          </li>
          <li><b>username</b>
          </li>
          <li><b>password</b>
          </li>
          <li><b>client_id</b>
          </li>
        </ul>
      </td>
      <td style="text-align:left">
        <p>Dictionary of the user credentials</p>
        <ul>
          <li>Type of authentication to be used. (e.g. password)</li>
          <li>User name to be used for authentication</li>
          <li>Password to be used for authentication</li>
          <li>Client ID to be used for authentication (e.g. profile-api, belt-api)</li>
        </ul>
      </td>
      <td style="text-align:left">-</td>
    </tr>
    <tr>
      <td style="text-align:left"><b>api_urls</b>
      </td>
      <td style="text-align:left">map of api endpoints to their base urls. <code>fallback</code> will be used
        in case the api (key) is not in the map.</td>
      <td style="text-align:left">
        <p><code>{&quot;harvester-api&quot;:token_url,</code>
        </p>
        <p><code>&quot;belt-api&quot;:token_url,</code>
        </p>
        <p><code>...,</code>
        </p>
        <p><code>&quot;fallback&quot;:token_url}</code>
        </p>
      </td>
    </tr>
    <tr>
      <td style="text-align:left"><b>token_url</b>
      </td>
      <td style="text-align:left">fallback URL of the API to be used.</td>
      <td style="text-align:left"><code>&quot;URL&quot;</code>
      </td>
    </tr>
    <tr>
      <td style="text-align:left"><b>destfile_profilestore</b>
      </td>
      <td style="text-align:left">Destination file to be used to store the export of the Profile Store.</td>
      <td
      style="text-align:left"><code>profileStoreExport.csv</code>
        </td>
    </tr>
    <tr>
      <td style="text-align:left"><b>destfile_eventstore</b>
      </td>
      <td style="text-align:left">Destination File to be used to store the export of the Event Store.</td>
      <td
      style="text-align:left"><code>eventStoreExport.csv</code>
        </td>
    </tr>
    <tr>
      <td style="text-align:left"><b>delimiter</b>
      </td>
      <td style="text-align:left">Delimiter to be used in the CSV files.</td>
      <td style="text-align:left"><code>\t</code>
      </td>
    </tr>
    <tr>
      <td style="text-align:left"><b>profileTypes</b>
      </td>
      <td style="text-align:left">List of profileTypes the Report should be restricted to.</td>
      <td style="text-align:left"><code>[]</code> (i.e. all profile types)</td>
    </tr>
    <tr>
      <td style="text-align:left"><b>correlationIds</b>
      </td>
      <td style="text-align:left">List of correlationIds the Report should be restricted to.</td>
      <td style="text-align:left"><code>[]</code> (i.e. use one sample Correlation ID per profile type)</td>
    </tr>
  </tbody>
</table>

### Example Configuration

```text
credentials = {
    "grant_type":"password",
    "username":"USERNAME",
    "password":"PASSWORD",
    "client_id":"profile-api"
}
token_url = "URL"

api_urls = {
    "belts": token_url,
    "harvesters": token_url,
    "profiles": token_url,
    "events": token_url,
    "event-types": token_url,
    "fallback": token_url # Prevents Errors on changing/extending API's
                          # Defaults e.g. to token_url. 
}

destfile_profilestore = "profileStoreExport.csv"
destfile_eventstore = "eventStoreExport.csv"
delimiter = "\t"
profileTypes = []
correlationIds = []
```

## Example output

```text
harvester    eventType               beltId    beltName          correlationId                                                                           profileType                      path
-----------  ------------------      --------  ----------------  --------------------------------------------------------------------------------------  -------------------------------  -----------------------------------------------------------------------
N/A          N/A                     N/A       N/A               data@grnry.io                                                                           account                          /FirstName
N/A          N/A                     N/A       N/A               data@grnry.io                                                                           account                          /_id@session@profile_store
N/A          N/A                     N/A       N/A               data@grnry.io                                                                           account                          /_id@session@profile_store
N/A          N/A                     N/A       N/A               data@grnry.io                                                                           account                          /_id@session@profile_store
N/A          N/A                     N/A       N/A               data@grnry.io                                                                           account                          /LastName
N/A          N/A                     N/A       N/A               data@grnry.io                                                                           account                          /Phone
N/A          N/A                     N/A       N/A               data@grnry.io                                                                           account                          /salesforceid
N/A          N/A                     N/A       myorigin          ProfileA                                                                                agency                           /lengths/2018-07
N/A          N/A                     N/A       myorigin          ProfileA                                                                                agency                           /lengths/2018-08
N/A          N/A                     N/A       myorigin          ProfileA                                                                                agency                           /lengths/2018-09
N/A          N/A                     N/A       myorigin          ProfileA                                                                                agency                           /visits/2018-07
N/A          N/A                     N/A       myorigin          ProfileA                                                                                agency                           /visits/2018-08
N/A          N/A                     N/A       myorigin          ProfileA                                                                                agency                           /visits/2018-09
N/A          N/A                     N/A       myorigin          ProfileA                                                                                agency                           /visits/2019-10
N/A          N/A                     N/A       myorigin          ProfileA                                                                                agency                           /visits/2019-11
N/A          N/A                     N/A       myorigin          ProfileA                                                                                agency                           /visits/2019-12
N/A          N/A                     N/A       N/A               2097-1904                                                                               Airports                         /airports
N/A          N/A                     N/A       integration-test  e2e-sessionizing-test_da43995b-60ec-4128-837a-8a30008155f0-1576016709266-1576016800289  e2e-sessionizing-test            /number_of_messages
snowplow-catch-all	snowplow-catch-all	634	    536-test-belt	    a8df2c	                                                                                test-536	                    /opensky
snowplow-catch-all-	snowplow-catch-all	634	    536-test-belt	    a8df2c	                                                                                test-536	                    /opensky
```

