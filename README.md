# Congestion router

A web page that compares HERE's fastest driving route with the least congested route within a set extra-time limit. It is a proof of concept for routing around live traffic in London, preferring uncongested roads even when the route is longer.

## How it works

1. Requests HERE's fastest route and up to five alternatives (Routing API v8).
2. Fetches live traffic flow for the area around those routes (Traffic API v7) and measures how much of each route runs through congestion.
3. Requests new routes that avoid the congested stretches, for up to three rounds.
4. Picks the route with the least congestion within the extra-time limit.

## Use

Open the page, enter a HERE API key under Settings, and enter a start and destination. The key is kept only in the browser that entered it; it is not stored in this repository.

On an iPhone, open the GitHub Pages address in Safari and choose Share, then Add to Home Screen.

## Road preference

Under Settings, "Road preference" can favour smaller roads. It caps the speed HERE assumes on main roads (functional classes 1 and 2, and class 3 on the strong setting) for the extra routes the app requests, and adds a cost per kilometre of main road when choosing between routes. The plain fastest route is always requested without caps, so its time is real; times for capped routes may be overstated where they use main roads.

## Base map

The map background uses Tracestrack Topo by default. It needs a Tracestrack API key, entered under Settings; Tracestrack's free plan is for non-commercial use and allows 100,000 tile requests a month. Until a key is entered, OpenStreetMap's standard tiles are shown. HERE's own tiles can also be chosen, and count against the HERE allowance. OpenStreetMap's tiles must be requested with a Referer header, so they load from the hosted page but not from a copy of the file opened directly from disk.

## Favourites and recalculating

Locations can be saved as favourites with "+ Save favourite" under the start and destination. They are kept as names and coordinates in the browser's local storage and can be deleted under Settings. The button at the top left of the map runs the last comparison again with the same inputs, using live traffic at that moment.

## London features

- **TfL disruptions.** Active disruptions from TfL's Unified API are drawn on the map and counted against each route; routes through a TfL road closure are avoided where possible. The data is fetched at most every 5 minutes, and without a TfL app key at most 400 times a day.
- **Charges.** Each route shows its toll and charge-zone costs from HERE (the Congestion Charge and the Blackwall and Silvertown tunnels, for example). HERE counts a request that returns charges as two transactions, and the app counts it that way too. Settings can avoid the Congestion Charge zone or charged tunnels and toll roads.
- **Recalculate from here.** With your location shown, the recalculate button starts from your current position.
- **Re-check while driving.** Optional. Every 5, 10 or 15 minutes, while your location is shown and the app is open, the app compares the rest of the current route with routes from your position and offers to switch if one is clearly better. Each check uses at most 3 HERE requests (without charges); there are at most 12 checks a day and 150 a month, and checks pause once HERE routing or traffic use passes half its monthly allowance.
- **Journey log.** When you set off from the start with your location shown and arrive at the destination, the trip's actual time, predicted time and the route followed are logged in the browser. The log can be copied as CSV from Settings.
- **Shortcuts.** A compared journey can be saved as a one-tap shortcut that fills both ends and runs the comparison.
- **Dark map.** The map can be darkened with the phone's dark mode or after sunset.

HERE routing already applies time-dependent restrictions, such as school streets, for the current time, since route calculations are time-aware by default.

## Location and screen

The location button under the settings cog shows your live position on the map. The first tap shows the position and keeps the map centred on it; dragging the map stops the centring, and tapping again re-centres. Tapping while it is centred turns it off. "Keep the screen on" under Settings stops the phone dimming and locking while the app is open, using the Screen Wake Lock API (iOS 18.4 or later for home-screen apps).

## Free allowance limit

The page counts its own requests per service in the browser's local storage and refuses a request that would pass the chosen share of HERE's free allowance. Requests made from other browsers or devices are not included in the count.

## Known limitations

- Avoid areas apply to both directions of a road, so a jam on one carriageway also excludes the other.
- Traffic data is fetched only at jam factor 2 and above; anything lower counts as free-flowing.
