# Congestion router

A web page that compares HERE's fastest driving route with the least congested route within a set extra-time limit. It is a proof of concept for routing around live traffic in London, preferring uncongested roads even when the route is longer.

## How it works

1. Requests HERE's fastest route and up to five alternatives (Routing API v8).
2. Fetches live traffic flow for the area around those routes (Traffic API v7) and measures how much of each route runs through congestion.
3. Requests new routes that avoid the congested stretches, for up to three rounds.
4. Picks the route with the least congestion within the extra-time limit.

## Use

Open the page, enter a HERE API key under "HERE API key", and enter a start and destination. The key is kept only in the browser that entered it; it is not stored in this repository.

On an iPhone, open the GitHub Pages address in Safari and choose Share, then Add to Home Screen.

## Free allowance limit

The page counts its own requests per service in the browser's local storage and refuses a request that would pass the chosen share of HERE's free allowance. Requests made from other browsers or devices are not included in the count.

## Known limitations

- Avoid areas apply to both directions of a road, so a jam on one carriageway also excludes the other.
- Traffic data is fetched only at jam factor 2 and above; anything lower counts as free-flowing.
