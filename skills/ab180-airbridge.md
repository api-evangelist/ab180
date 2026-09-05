---
name: Airbridge
description: Use when setting up mobile app attribution and measurement, creating tracking links for ad campaigns, integrating with ad channels (Google Ads, Meta, Apple Ads, TikTok), configuring SDKs for event tracking, analyzing campaign performance through reports, or implementing deep linking and SKAdNetwork for iOS measurement.
metadata:
    mintlify-proj: airbridge
    version: "1.0"
---

# Airbridge Skill

## Product summary

Airbridge is a mobile measurement partner (MMP) that tracks ad performance across apps and websites through a unified dashboard. Agents use Airbridge to measure attribution, create tracking links, integrate with ad channels, configure SDKs for event collection, and analyze campaign performance via reports. Key files: API tokens stored in [Settings]>[Tokens]; app configuration in [Settings]>[App Settings]; tracking links created in [Tracking Link]>[Link Generation]. Primary API endpoint: `https://api.airbridge.io`. See [Airbridge Help Center](https://help.airbridge.io) for full documentation.

## When to use

Reach for this skill when:
- A user needs to set up mobile app attribution and measurement
- Creating or managing tracking links for ad campaigns
- Integrating with ad channels (Google Ads, Meta Ads, Apple Ads, TikTok, or 50+ others)
- Implementing Airbridge SDK for event tracking (Android, iOS, React Native, Flutter, Unity, Web)
- Analyzing campaign performance through Actuals Reports, retention reports, or revenue reports
- Setting up deep linking for user engagement
- Configuring SKAdNetwork (SKAN) for iOS campaign measurement
- Troubleshooting data discrepancies between Airbridge and ad channels
- Exporting raw data or integrating with third-party platforms (GA4, Mixpanel, Braze, etc.)

## Quick reference

### Core workflows

| Task | Location | Key steps |
|------|----------|-----------|
| Register app | Dashboard > [Settings] > [App Settings] | Add app name, select timezone, set currency |
| Create tracking link | [Tracking Link] > [Link Generation] | Select channel (integrated or custom), set destination (app/store/web), add parameters |
| Integrate ad channel | [Integrations] > [Ad Channel Integration] | Connect Google Ads, Meta, Apple Ads, TikTok, etc. via OAuth or API keys |
| View performance | [Reports] > [Actuals Report] | Set date range, select metrics, apply GroupBys, filter by channel/campaign |
| Install SDK | Developer docs | Choose platform (Android/iOS/React Native/etc.), follow SDK quickstart |
| Set attribution rules | [Management] > [Attribution Rules] | Configure lookback window (default 7 days), attribution window (default 30 days) |

### API authentication

All API requests require:
- **Endpoint**: `https://api.airbridge.io`
- **Header**: `Authorization: Bearer {API-TOKEN}`
- **Header**: `Content-Type: application/json; charset=utf-8`
- **Token location**: Dashboard > [Settings] > [Tokens]
- **Token types**: API Token (all endpoints), Tracking Link API Token (link creation only)

### Event types

| Category | Examples |
|----------|----------|
| Standard Events | SIGN_UP, SIGN_IN, PRODUCT_VIEWED, ADD_TO_CART, ORDER_COMPLETED, SUBSCRIBE |
| Custom Events | User-defined events (e.g., `achieve_level_10`, `tutorial_complete`) |
| Auto-collected | Country, app version, device model, OS name/version |

### Tracking link destinations

| Destination | Use case | Platforms |
|-------------|----------|-----------|
| App (Deep Link) | Route users to specific in-app page | Android, iOS, Desktop |
| App Store | Route to app store listing | Google Play, Apple App Store, Web URL |
| Website | Route to web URL | All platforms |
| Airpage | Desktop landing page (custom channels only) | Desktop |

### Report types

| Report | Measures | Key metrics |
|--------|----------|-------------|
| Actuals Report | Real-time performance | Installs, events, revenue, users |
| Retention Report | User cohort retention | N-day retention rate, predictive lifetime |
| Revenue Report | Revenue by cohort | ARPU, ARPPU, predictive LTV |
| Funnel Report | Conversion flow | Step completion rate, drop-off |
| Active Users Report | DAU/MAU | Daily/monthly active users |

## Decision guidance

### When to use integrated channel vs. custom channel tracking links

| Scenario | Use integrated channel | Use custom channel |
|----------|----------------------|-------------------|
| Running ads on Google Ads, Meta, Apple Ads, TikTok | ✓ | |
| Using owned media (email, push, social) | | ✓ |
| Ad channel not in Airbridge's integration list | | ✓ |
| Need automatic cost data sync | ✓ | |
| Need manual parameter control | ✓ | ✓ |
| Want to exclude from attribution | | ✓ (use `ab_redirection_only`) |

### When to use tracking link vs. SDK event

| Data type | Use tracking link | Use SDK event |
|-----------|------------------|---------------|
| Ad impressions/clicks from external placements | ✓ | |
| In-app user actions (purchase, level achieved) | | ✓ |
| Web campaign tracking | ✓ | |
| Deep link attribution | ✓ | |
| Server-to-server events | | ✓ (via S2S API) |

### When to use different attribution windows

| Business model | Lookback window | Attribution window | Reason |
|----------------|-----------------|-------------------|--------|
| Gaming (high engagement) | 7 days | 30 days | Longer path to monetization |
| E-commerce (quick purchase) | 3 days | 7 days | Shorter decision cycle |
| Subscription (high LTV) | 7 days | 60+ days | Extended user value |
| Reactivation campaigns | 1 day | 7 days | Recent touchpoint priority |

## Workflow

### 1. Set up Airbridge for a new app

1. **Register app**: Go to Dashboard > [Settings] > [App Settings] > [App Info]. Enter app name, select timezone (cannot be changed later), set currency.
2. **Configure app**: Set app store URLs for iOS and Android in [Platform] section. Enable IDFA collection for iOS if using Growth plan.
3. **Set attribution rules**: Go to [Management] > [Attribution Rules]. Set lookback window (default 7 days) and attribution window (default 30 days) based on your business model.
4. **Generate API tokens**: Go to [Settings] > [Tokens]. Copy API Token for backend use and Tracking Link API Token for client-side link generation.

### 2. Integrate with ad channels

1. **Choose channel**: Go to [Integrations] > [Ad Channel Integration]. Select Google Ads, Meta, Apple Ads, TikTok, or other channel.
2. **Authenticate**: Click "Connect" and authorize via OAuth or enter API credentials (varies by channel).
3. **Enable cost integration** (optional): Toggle cost data sync to view ad spend in reports.
4. **Enable SKAN integration** (iOS only): Set up conversion values first in [Management] > [SKAdNetwork], then enable SKAN integration for the channel.
5. **Verify**: Check integration status in dashboard. Campaign data updates in real-time; cost data updates every 4 hours.

### 3. Create tracking links

1. **Choose channel type**: Go to [Tracking Link] > [Link Generation]. Select "Integrated Channels" (Google Ads, Meta, etc.) or "Custom Channels" (owned media, non-integrated channels).
2. **Set parameters**: Enter campaign, ad group, creative, and other parameters. These become URL parameters for reporting.
3. **Set destination**: Choose App (Deep Link), App Store, or Website. For deep links, enter URL scheme (e.g., `myapp://product/123`).
4. **Set fallback**: For deep links, specify where users without the app go (app store or web URL).
5. **Select events for attribution**: Choose "All Events" unless excluding installs or deep link events.
6. **Create link**: Click "Create tracking link". Airbridge generates Click, Impression, and S2S Click links (varies by channel).

### 4. Install SDK and track events

1. **Choose platform**: Select Android, iOS, React Native, Flutter, Unity, or Web from [Developer Guide].
2. **Install SDK**: Follow SDK quickstart. Add SDK dependency, initialize with app name and token.
3. **Track standard events**: Call `trackEvent()` with standard categories (e.g., `SIGN_UP`, `ORDER_COMPLETED`).
4. **Track custom events**: Define custom event names and semantic attributes (e.g., `achieve_level_10` with `level=10`).
5. **Configure deep linking**: Implement deep link handlers to route users to specific app pages.
6. **Test SDK**: Use debug logs to verify events are being sent. Check [Tracking Events with Real-time Logs] in dashboard.

### 5. View performance reports

1. **Open Actuals Report**: Go to [Reports] > [Actuals Report].
2. **Set date range**: Choose "Between", "Since", or "Last N days". Default timezone is app timezone.
3. **Select metrics**: Choose 1-20 metrics (e.g., Installs, Revenue, Users). Metrics without data don't appear.
4. **Apply GroupBys**: Select 0-10 dimensions (e.g., Channel, Campaign, Country) to break down metrics.
5. **Filter results**: Narrow by GroupBy values (e.g., "Channel is Meta Ads").
6. **Save report**: Click "Save to My Reports" to reuse settings. Create sharelink for CSV export.

### 6. Troubleshoot data discrepancies

1. **Check attribution rules**: Verify lookback and attribution windows match your expectations in [Management] > [Attribution Rules].
2. **Compare with ad channel**: Check if ad channel uses different attribution window (e.g., Meta uses 1-7 days; Airbridge uses 30 days by default).
3. **Review privacy policies**: Meta masks data if impressions < 1,000 or if you haven't agreed to Advanced Mobile Measurement (AMM) Terms.
4. **Check SKAN data**: iOS 14.5+ users without IDFA consent are measured via SKAN, which has different attribution rules.
5. **Verify postback setup**: Go to [Integrations] > [Postback Settings] to ensure events are being sent back to ad channels correctly.

## Common gotchas

- **App timezone is immutable**: Set timezone correctly during app registration. You cannot change it afterward.
- **Deep link encoding required**: URL scheme deep links must be properly encoded (e.g., `%3D` for `=`). Unencoded special characters break the link.
- **IDFA collection requires ATT prompt**: iOS apps must implement App Tracking Transparency (ATT) prompt. Set `setAutoDetermineTrackingAuthorizationTimeout` to 30+ seconds.
- **Attribution window affects all channels**: Changing attribution window applies globally. Different channels may have different optimal windows.
- **Meta privacy masking**: Data masked if impressions < 1,000 or AMM Terms not agreed. Appears as "Privacy Block" or `±α` notation.
- **Event timestamp must be recent**: Server-to-server events must have `eventTimestamp` within 24 hours of transmission. Older events are rejected.
- **Tracking link parameters are case-sensitive**: Parameter names become GroupBy values. Use consistent casing (e.g., `campaign` not `Campaign`).
- **Custom channel names have restrictions**: Only lowercase letters, numbers, `.`, `-`, `_`. Names must be unique.
- **Postback delays**: Cost data updates every 4 hours; SKAN data updates every 24 hours. Real-time data may lag.
- **Deprecated SDKs**: Android SDK v2, iOS SDK v1, React Native SDK v2 are deprecated. Use v4 versions.

## Verification checklist

Before submitting work with Airbridge:

- [ ] App is registered with correct timezone and currency
- [ ] API tokens are generated and stored securely (never expose in client code)
- [ ] Attribution rules (lookback/attribution windows) match business model
- [ ] Ad channels are integrated and showing "Connected" status
- [ ] Tracking links are created with correct destination (app/store/web)
- [ ] Deep links are properly URL-encoded if using App (Deep Link) destination
- [ ] SDK is installed and events are appearing in real-time logs
- [ ] Standard events are tracked (at least SIGN_UP, ORDER_COMPLETED)
- [ ] Custom events are defined with semantic attributes where applicable
- [ ] Actuals Report shows data for selected date range and metrics
- [ ] GroupBys and filters are applied correctly to reports
- [ ] Postback settings are configured for ad channel optimization
- [ ] SKAN conversion values are set up if running iOS campaigns
- [ ] Raw data export or sharelink is working if needed for external use

## Resources

- **Comprehensive page listing**: [https://help.airbridge.io/llms.txt](https://help.airbridge.io/llms.txt)
- **Getting Started**: [Onboarding Airbridge](https://help.airbridge.io/en/guides/getting-started-with-airbridge)
- **API Reference**: [Airbridge API Introduction](https://help.airbridge.io/en/references/introduction)
- **SDK Integration**: [Getting Started with Airbridge SDK](https://help.airbridge.io/en/developers/airbridge-sdk-overview)

---

> For additional documentation and navigation, see: https://help.airbridge.io/llms.txt