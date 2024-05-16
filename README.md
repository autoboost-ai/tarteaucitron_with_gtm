# Tarteaucitron.js + Google Consent Mode V2

This repository contains Tarteaucitron 1.17 designed to load GTM with limited permissions under No Consent mode.

---

### *Enable* Limited GTM Access:

Provide the option `gtmWithLimitedPermissions: true` inside your init({..})

Then you will also need:
- For GTM support add: `gtagUa: "12345-XXX"`
- For Facebook Pixel integration, you will also need to provide `facebookPixelId: "12345677890"`


### _(Optional)_ Serving GTM.js from your own custom tagging server / domain
- Add  `gtagUrl: "https://yourserver.com/gtm.js?id=GTM-XXXX" }`

Otherwise the Tarteaucitron.js Gtag service will load the GTM.js from: `https://www.googletagmanager.com/gtag/js?id=GTM-XXXX` by default.


----

# Full Tarteaucitron.js configuration
```
tarteaucitron.init({
  "privacyUrl": "",    /* Privacy policy url */ 
  "highPrivacy": true,    /* Disable auto consent */ 
  "hashtag": "#tarteaucitron",    /* Open the panel with this hashtag */ 
  "cookieName": "tartaucitron",    /* Cookie name */ 
  "orientation": "bottom",    /* Banner position (top - bottom) */ 
  "showAlertSmall": false,    /* Show the small banner on bottom right */ 
  "cookieslist": false,    /* Show the cookie list */ 
  "adblocker": false,    /* Show a Warning if an adblocker is detected */ 
  "AcceptAllCta": true,   /* Show the accept all button when highPrivacy on */ 
  "handleBrowserDNTRequest": false,  /* If Do Not Track == 1, accept all */ 
  "removeCredit": false,   /* Remove credit link */
  "moreInfoLink": true,    /* Show more info link */ 
  "bodyPosition": "top",    /* Body position (top - bottom) */
  "googleConsentMode": true, /* Enable Google Consent Mode v2 for Google ads and GA4 */

  // NEW OPTIONS FOR GOOGLE CONSENT V2
  "gtmWithLimitedPermissions": true,  // required for GTM GoogleConsentV2 loading, otherwise normal tarteaucitron.js behaviour is executed.  
  "gtagUa": "GTM-XXXX", // [required if gtmWithLimitedPermissions is true]
  "gtagUrl": "https://gtm-tagging.yourdomain.com/gtm.js?id=GTM-XXXX" // [optional]
  "facebookPixelId": "1234567890", // [required if want fb pixel tracking]
  "anonymize_ip_always": false, // [optional] optionally enforce ip anonymization even with consent
  "anonymize_ip_if_only_for_no_consent: true // [optional] only anonymize the ip if the user does not give tracking consent
});
```

---

# How it works
1. if `gtmWithLimitedPermissions` is set to `true`, the Tarteaucitron.js will load the GTM.js from your own custom tagging server / domain or the default `https://www.googletagmanager.com/gtag/js?id=GTM-XXXX` if `gtagUrl` is not set.
2. Then the Tarteaucitron.js will detect if the settings for "googletagmanager=true"
3. If the settings for "googletagmanager=true" is set, the Tarteaucitron.js tell Gtag.js that permissions are granted.
4. If the settings for "googletagmanager=true" is not set, the Tarteaucitron.js tell Gtag.js that permissions are not granted.
5. Regardless of the consent, GTM will be loaded.

The goal is to limit Gtag.js data collection when the user does not give consent to tracking.
This is done by `gtag('consent', 'update', {'ad_storage': 'denied', 'analytics_storage': 'denied'});`