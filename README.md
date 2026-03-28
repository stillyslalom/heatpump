# Heat Pump vs Gas Furnace Calculator

[Live app](https://stillyslalom.github.io/heatpump/)

A single-page webapp that estimates whether a homeowner should stick with a gas furnace, switch to a heat pump, or run both as a hybrid system. It needs only a ZIP code and two utility bills.

## How it works

Enter a ZIP code and the app auto-fills state-average energy rates and usage from EIA data. For a more precise estimate, replace the defaults with values from your actual January (peak heating) and May (baseline) utility bills. From these two bills, the calculator derives gas and electric volumetric rates, separates baseline gas usage (water heater, stove, dryer) from heating gas, and notes the billing period dates.

Historical daily temperatures for the January billing period are fetched from NOAA via the [Open-Meteo](https://open-meteo.com/) archive API. The heating degree days (HDD) for that period, combined with the heating gas consumed, yield the home's thermal load factor in BTU per degree-day — a measure of how much heat the building loses for each degree of outdoor cold. Five years of daily temperatures are then fetched to determine the design temperature (5th percentile of heating-season daily lows across all five years), auto-select a standard or cold-climate COP curve, and project day-by-day heating costs across all three scenarios. Costs are averaged over the five-year window; the worst and mildest winters are reported for context.

The three scenarios are gas only, heat pump only, and hybrid. The hybrid approach uses the heat pump above a crossover temperature — the outdoor temp where heat pump cost per BTU equals gas furnace cost per BTU — and the gas furnace below it. This crossover is derived analytically from the gas rate, electric rate, furnace efficiency, and COP curve. In cold climates with cheap gas, the hybrid is often the most economical option, and it also provides cooling.

A second results stage estimates installation costs and simple payback for both a full heat pump replacement and a hybrid add-on (keep the furnace, add a heat pump for mild-weather heating and cooling). These estimates adjust based on the user's existing cooling system, equipment age, electrical panel amperage, and ductwork condition.

An expandable advanced billing section lets users model tiered/inclining block rates, time-of-use pricing, demand charges, and solar net metering. These options are combinable — for example, TOU rates and demand charges can be active simultaneously.

## Data sources and limitations

State-level energy rates and residential usage come from the [EIA](https://www.eia.gov/) Electric Power Monthly and Natural Gas Monthly (embedded 2024-2026 snapshot). Historical daily temperatures are fetched live from the [Open-Meteo Archive API](https://open-meteo.com/en/docs/historical-weather-api), which wraps NOAA station data. ZIP code geocoding uses [Zippopotam.us](https://api.zippopotam.us/).

The comparison covers heating only; cooling savings from replacing an AC with a heat pump are noted but not modeled. Cost projections are averaged over five years of weather data, and the worst winter in that window is reported separately so users can gauge year-to-year variability. The design temperature used for system sizing is the 5th percentile of heating-season daily lows across the full five-year pool, so it reflects the worst observed conditions. COP curves are representative of modern equipment but not tied to any specific brand or model — real performance depends on installation quality, duct sizing, and refrigerant charge. Federal incentives are not modeled because the landscape is in flux; the app suggests checking state and utility programs. The calculator assumes gas service remains connected for other appliances, so fixed gas charges are the same across all scenarios.

The app is a single `index.html` with no build step or dependencies. Open it in a browser.
