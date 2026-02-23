Urban Heat/Cold Vulnerability Assessment System

This system provides real-time assessment of heat and cold vulnerability across Manhattan's census tracts by combining temperature monitoring, demographic data, and statistical analysis.

Research Contributions:
1. Z-score based deviation detection using historic baseline (mean/stdDev from cumulative observations)
2. Composite vulnerability metric weighted 60% temperature deviation, 40% elderly population
3. Multi-threshold risk classification: Extreme (1.5σ), Very High (1.2σ), High (0.9σ), Elevated (0.5σ)
4. Cumulative risk analysis identifying persistent hot/cold spots over time

Vulnerability Risk Score:
Z-score normalization: Z = (current_temp - historic_mean) / historic_stdDev
Normalize elderly population: E = tract_elderly_count / max_elderly_count
Composite risk score: Risk = (Z × 0.6) + (E × 0.4)

Risk Classification:
Extreme: Score ≥ 1.5 (95th+ percentile) - Dark Red/Dark Blue
Very High: Score 1.2-1.5 (85th-95th percentile) - Orange Red/Medium Blue
High: Score 0.9-1.2 (75th-85th percentile) - Orange/Royal Blue
Elevated: Score 0.5-0.9 (65th-75th percentile) - Gold/Sky Blue

Heat layers only display tracts with Z ≥ +0.5σ (above-average temperatures). Cold layers only display tracts with Z ≤ -0.5σ (below-average temperatures).

System Architecture
- index.html (web application interface)
- tempage.js (core algorithms - academic version)
- get_latest_temperatures.php (API endpoint for current observations)
- get_risk_statistics.php (API endpoint for statistics and baseline)
- 2020tractsage_with_age.geojson (census tracts with demographics)

Quick Start
Prerequisites: Web server (Apache/Nginx), PHP 7.4+, MySQL 5.7+, OpenWeatherMap API key

API Endpoints
1. Get Latest Temperatures
GET get_latest_temperatures.php

Returns: Current temperature observations for all census tracts

Response format: JSON object with success flag, data_count (288), last_update timestamp, and data array containing tract_id, tract_name, temperature_f, temperature_c, humidity, conditions, elderly_65plus, observed_at, and other fields for each census tract.

2. Get Risk Statistics
GET get_risk_statistics.php

Returns: Historic baseline, cumulative statistics, vulnerability counts

Response format: JSON object with success flag, global_averages (temperature and stddev_temp - used for z-score calculations), heat_vulnerability (extreme_count, extreme_elderly, very_high_count, etc.), cold_vulnerability (same structure), cumulative_heat_stats and cumulative_cold_stats (persistent pattern analysis), and tracts array with cumulative_heat_risk (average z-score), heat_frequency (proportion of time above +0.5σ), cumulative_cold_risk, and cold_frequency for each tract.
