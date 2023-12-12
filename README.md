# react-native-apple-ads-attribution

Fetches apple attribution data via iAd and AdServices APIs

## Requirements
- iAd Framework
- AdServices Framework

## Installation

```sh
npm install @nutrisense/react-native-apple-ads-attribution
```

## Usage

```js
import AppleAdsAttribution from "@nutrisense/react-native-apple-ads-attribution";

const attributionData = await AppleAdsAttribution.getAttributionData();
const iAdAttributionData = await AppleAdsAttribution.getiAdAttributionData();
const adServicesAttributionToken = await AppleAdsAttribution.getAdServicesAttributionToken();
const adServicesAttributionData = await AppleAdsAttribution.getAdServicesAttributionData();
```
Note from version 1.0 the above functions propagates errors, so they should be wrapped in try/catch.

## Documentation

#### getAttributionData()
Gets install attribution data first trying to use the AdServices API (iOS 14.3+).  
If it fails to retrieve data it will fallback to iAd API.
Throws error if everything fails

#### getiAdAttributionData()
Gets install attribution data using iAd API https://developer.apple.com/documentation/iad/setting_up_apple_search_ads_attribution/  
Throws error if data couldn't be retrieved (e.g. if user rejected permission for app tracking)

##### Example
```javascript
try {
  const iAdAttributionData = await AppleAdsAttribution.getiAdAttributionData()
  console.log(iAdAttributionData) // -->
    // {
    //   "Version3.1": {
    //     "iad-lineitem-name": "LineName",
    //     "iad-attribution": "true",
    //     "iad-campaign-name": "CampaignName",
    //     "iad-org-name": "OrgName",
    //     "iad-conversion-type": "Download",
    //     "iad-org-id": "1234567890",
    //     "iad-campaign-id": "1234567890",
    //     "iad-adgroup-name": "AdGroupName",
    //     "iad-country-or-region": "US",
    //     "iad-creativeset-name": "CreativeSetName",
    //     "iad-keyword-matchtype": "Broad",
    //     "iad-conversion-date": "2021-04-07T12:14:04Z",
    //     "iad-creativeset-id": "1234567890",
    //     "iad-keyword-id": "12323222",
    //     "iad-lineitem-id": "1234567890",
    //     "iad-click-date": "2021-04-07T12:14:04Z",
    //     "iad-purchase-date": "2021-04-07T12:14:04Z",
    //     "iad-adgroup-id": "1234567890",
    //     "iad-keyword": "Keyword"
    //   }
    // }
} catch (error) {
  const { message } = error
  console.log(message) // --> Some error message
}
```

#### getAdServicesAttributionToken()
Generates a AdServices token valid for 24 hours that then can be used to request attribution data from Apples AdServices API, see https://developer.apple.com/documentation/adservices  
Throws error if token couldn't be generated

##### Example
```javascript
try {
  const adServicesAttributionToken = await AppleAdsAttribution.getAdServicesAttributionToken()
  console.log(adServicesAttributionToken) // -->
    // KG6LvapO97gCkOfUqi412/n8v8fu8AMTB.....
} catch (error) {
  const { message } = error
  console.log(message) // --> Some error message
}
```

#### getAdServicesAttributionData()
Generates a AdServices token and uses it to request attribution data from Apples AdServices API, see https://developer.apple.com/documentation/adservices  
Throws error if data couldn't be fetched.

##### Example
```javascript
try {
  const adServicesAttributionData = await AppleAdsAttribution.getAdServicesAttributionData()
  console.log(adServicesAttributionData) // -->
    // {
    //   "keywordId": 12323222,
    //   "campaignId": 1234567890,
    //   "conversionType": "Download",
    //   "creativeSetId": 1234567890,
    //   "orgId": 1234567890,
    //   "countryOrRegion": "US",
    //   "adGroupId": 1234567890,
    //   "clickDate": "2021-04-08T07:45Z",
    //   "attribution": true
    // }
catch (error) {
  const { message } = error
  console.log(message) // --> Some error message
}
```

## License

MIT
