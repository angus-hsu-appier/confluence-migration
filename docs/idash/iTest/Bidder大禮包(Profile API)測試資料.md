---
title: Bidder大禮包(Profile API)測試資料
source: https://appier.atlassian.net/wiki/spaces/IDASH/pages/4880039955/Bidder+Profile+API
confluence_id: 4880039955
space: IDASH
last_modified: 2026-03-03
author: Angus Hsu
migrated_at: 2026-06-03
---

# Bidder大禮包(Profile API)測試資料

!!! info "Endpoint"
    - **Staging:** `https://be.idash-staging.appier.org/mars/restful/v0/traffic_deployments/{cid}`
    - **Production:** `https://legacy.idash.appier.org/mars/restful/v0/traffic_deployments/{cid}`

## Test Campaigns

| **OID**<br>*Ad Solution (promotion content + MMP)* | **CID**<br>*Integration Type* | **Description** | **Owner** |
|---|---|---|---|
| [DO NOT USE] QA_api_test_RTB_AIBID<br>[gGdJnW5RTT6Wlngb8yVg4A](https://idash.appier.org/campaign-list/#/list/KHv00GW0QA6xg7RhGffNlA/gGdJnW5RTT6Wlngb8yVg4A)<br>*AIBID (iOS + Adjust)* | [DO NOT USE] UNI_QA_api_test_RTB_AIBID<br>[Wyam1x__RZOy_2tOjnInpA](https://idash.appier.org/irobot3-adgroup/#/list/gGdJnW5RTT6Wlngb8yVg4A/Wyam1x__RZOy_2tOjnInpA)<br>*RTB/Open RTB (Mopub, Mobclix, Aprax...)* | Ad group 所有 attributes 都有設定測試值 (除了 Ad Group Set Label & Target Product Set)<br>AI Model Setting: Whale / CPA | TDY |
| [DO NOT USE] QA_api_test_SKAN<br>[iAS3dLJHRHab6sZ4xnXKdQ](https://idash.appier.org/campaign-list/#/list/KHv00GW0QA6xg7RhGffNlA/iAS3dLJHRHab6sZ4xnXKdQ)<br>*AIBID (iOS + Appsflyer)* | [DO NOT USE] UNI_QA_api_test_SKAN<br>[eM24FOLuQJSxI7TlSspqtg](https://idash.appier.org/irobot3-adgroup/#/list/iAS3dLJHRHab6sZ4xnXKdQ/eM24FOLuQJSxI7TlSspqtg)<br>*RTB/Open RTB (Mopub, Mobclix, Aprax...)* | AI Model Setting: Fix Price / CPM | TDY |
| [DO NOT USE] QA_api_test_RTB_Aictivate<br>[FenDas2dQjik10IsDLlDPg](https://idash.appier.org/campaign-list/#/list/KHv00GW0QA6xg7RhGffNlA/FenDas2dQjik10IsDLlDPg)<br>*Aictivate (Android + Appsflyer)* | [DO NOT USE] UNI_QA_api_test_RTB_Aictivate<br>[CEjivRWQQ_eeVFZmsdnFeg](https://idash.appier.org/irobot3-adgroup/#/list/FenDas2dQjik10IsDLlDPg/CEjivRWQQ_eeVFZmsdnFeg)<br>*RTB/Open RTB (Mopub, Mobclix, Aprax...)* | AI Model Setting: Whale / CPA | TDY |
| [DO NOT USE] QA_api_test_RTB_Engagement<br>[gqVJcZktT8KrKfwgHeiJZg](https://idash.appier.org/campaign-list/#/list/KHv00GW0QA6xg7RhGffNlA/gqVJcZktT8KrKfwgHeiJZg)<br>*App Engagement (Android + Others)* | [DO NOT USE] UNI_QA_api_test_RTB_Engagement<br>[CGt-XE_7QBSYJBR9B4QlRg](https://idash.appier.org/irobot3-adgroup/#/list/gqVJcZktT8KrKfwgHeiJZg/CGt-XE_7QBSYJBR9B4QlRg)<br>*RTB/Open RTB (Mopub, Mobclix, Aprax...)* | AI Model Setting: Fix Price / CPM | TDY |
| [DO NOT USE] QA_api_test_Feed<br>[ylwiTh1WRwGEzP3SReEV3Q](https://idash.appier.org/campaign-list/#/list/KHv00GW0QA6xg7RhGffNlA/ylwiTh1WRwGEzP3SReEV3Q)<br>*App Install (Android + CAF (Adjust))* | [DO NOT USE] Upsmobi3_QA_api_test_Feed<br>[_adxbgtuQ6-mXu6fGAb0_A](https://idash.appier.org/irobot3-adgroup/#/list/ylwiTh1WRwGEzP3SReEV3Q/_adxbgtuQ6-mXu6fGAb0_A)<br>*Feed/upsmobi3* | AI Model Setting: n/a | TDY |
| [DO NOT USE] QA_api_test_RTB_Branding<br>[IDWj2uPPTlKS0Qx5879ORQ](https://idash.appier.org/campaign-list/#/list/KHv00GW0QA6xg7RhGffNlA/IDWj2uPPTlKS0Qx5879ORQ)<br>*Brand Awareness (Web)* | [DO NOT USE] UNI_QA_api_test_RTB_Branding<br>[JFIRsTDWQtyb_YghHdmyiQ](https://idash.appier.org/irobot3-adgroup/#/list/IDWj2uPPTlKS0Qx5879ORQ/JFIRsTDWQtyb_YghHdmyiQ)<br>*RTB/Open RTB (Mopub, Mobclix, Aprax...)* | AI Model Setting: Whale / CPA | TDY |
| [DO NOT USE] QA_api_test_FB<br>[W_hxgS3HSPmAu--8cA8X8g](https://idash.appier.org/campaign-list/#/list/KHv00GW0QA6xg7RhGffNlA/W_hxgS3HSPmAu--8cA8X8g)<br>*Gillnet EC (Web)* | [DO NOT USE] FB_QA_api_test_FB<br>[fDL2lAtbR8-ghkj3s8CMhg](https://idash.appier.org/irobot3-adgroup/#/list/W_hxgS3HSPmAu--8cA8X8g/fDL2lAtbR8-ghkj3s8CMhg)<br>*API/Facebook* | Enable Facebook Traffic = ON (Facebook API)<br>AI Model Setting: n/a | TDY |
| [DO NOT USE] QA_api_test_RTB_Lead_Gen<br>[g3sf6E1nTX2A9naTt1sEkg](https://idash.appier.org/campaign-list/#/list/KHv00GW0QA6xg7RhGffNlA/g3sf6E1nTX2A9naTt1sEkg)<br>*Lead Gen (Web)* | [DO NOT USE] UNI_QA_api_test_RTB_Lead_Gen<br>[BVk6dDF9QgCZDGAvDeOcDg](https://idash.appier.org/irobot3-adgroup/#/list/g3sf6E1nTX2A9naTt1sEkg/BVk6dDF9QgCZDGAvDeOcDg)<br>*RTB/Open RTB (Mopub, Mobclix, Aprax...)* | AI Model Setting: Whale / CPC | TDY |

!!! info
    **Profile API test data in TDX**

    - [https://appier.atlassian.net/wiki/spaces/PH/pages/3866853842](https://appier.atlassian.net/wiki/spaces/PH/pages/3866853842)

    **Profile API test data in TDZ**

    - TBC
