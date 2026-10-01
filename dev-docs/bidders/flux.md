---
layout: bidder
title: Flux
description: Flux Bidder Adapter
biddercode: flux
tcfeu_supported: true
dsa_supported: false
gvl_id: 1654
usp_supported: true
coppa_supported: true
schain_supported: true
dchain_supported: false
userId: all
media_types: banner
safeframes_ok: false
deals_supported: false
floors_supported: true
fpd_supported: true
pbjs: true
pbs: false
prebid_member: false
multiformat_supported: will-bid-on-any
ortb_blocking_supported: false
privacy_sandbox: no
sidebarType: 1
---

## Registration

The Flux bidding adapter requires setup before beginning. Please contact us at [bidder@fluxtech.ai](mailto:bidder@fluxtech.ai) to get a publisher ID.

## Description

The Flux adapter sends a single OpenRTB 2.6 bid request per auction to Flux's auction endpoint and supports banner ads only. Along with Prebid's standard first-party data, the request carries:

- TCF consent (`regs.gdpr`, `user.consent`; Flux is IAB TCF vendor 1654), US Privacy (`regs.us_privacy`), GPP (`regs.gpp`, `regs.gpp_sid`) and COPPA (`regs.coppa`) signals,
- the supply chain object (`source.schain`),
- user IDs from any configured User ID module (`user.eids`),
- the page section from `ortb2.site.ext.data.section` or an `article:section` meta tag, and keywords from `article:tag` meta tags when none are configured,
- `device.devicetype` and, where the browser exposes it, `device.connectiontype`.

Bids are returned in EUR, net revenue. Flux creatives are not compatible with SafeFrames; render them in a friendly iframe.

## Bid Params

{: .table .table-bordered .table-striped }
| Name          | Scope    | Description                                                         | Example         | Type     |
|---------------|----------|---------------------------------------------------------------------|-----------------|----------|
| `publisherId` | required | Publisher ID assigned by Flux                                       | `'11111'`       | `string` |
| `siteDomain`  | optional | Site domain. Only needed if different from the current page domain  | `'example.com'` | `string` |
| `placementId` | optional | Placement ID. Only needed if a placement differs from the others    | `'11111'`       | `string` |

## Example

```javascript
var adUnits = [
  {
    code: 'flux-banner-ad',
    mediaTypes: {
      banner: {
        sizes: [[300, 250]]
      }
    },
    bids: [
      {
        bidder: 'flux',
        params: {
          publisherId: 'test'
        }
      }
    ]
  }
];
```

## Floors

Flux supports the Price Floors module: it only bids at or above `imp.bidfloor`. Flux compares floors in EUR and skips floors set in another currency, so configure the Currency module if your floors aren't in EUR.

## User Sync

User sync is iframe-only. Allow iframe syncs for Flux in your Prebid config:

```javascript
pbjs.setConfig({
  userSync: {
    filterSettings: {
      iframe: {
        bidders: ['flux'],
        filter: 'include'
      }
    }
  }
});
```

## Testing

Add `fluxTest=1` to the page URL to receive a test bid. Test bids carry the ad server targeting key `fluxBidderTest=1`.
