# ViewDeck — Self-Hosted Weather Dashboard

> A beautiful, Apple Weather-inspired weather dashboard designed for home lab screens, Cozyla panels, and self-hosted dashboards.

---

<img src="https://github.com/user-attachments/assets/f4840205-e1c7-4b8c-96af-915236e2a440" alt="ViewDeck Dashboard - Main" width="100%">

<table>
  <tr>
    <td><img src="https://github.com/user-attachments/assets/b5441040-e75f-45b2-a77a-218710bb86d9" alt="ViewDeck Detail View" width="270"></td>
    <td><img src="https://github.com/user-attachments/assets/ddca35d8-4b67-4b65-b7d1-121b87fba710" alt="ViewDeck Theme 1" width="270"></td>
    <td><img src="https://github.com/user-attachments/assets/f856dcb1-892a-4598-a867-b58f2c53d09a" alt="ViewDeck Theme 2" width="270"></td>
  </tr>
</table>

---

## Overview

**ViewDeck** is a fully self-hosted, single-page weather dashboard served via Docker and Nginx. It pulls live data from OpenWeatherMap's One Call 3.0 API and renders a polished, responsive interface with glassmorphism aesthetics. Built for always-on home lab displays, Cozyla-style dashboards, wall-mounted screens, and smart home panels.

---

## Features

- **Live weather data** — current conditions, 24-hour hourly forecast, 10-day daily forecast
- **Precipitation** — next-hour bar chart (12 one-minute bars) + 6-hour historical + 24-hour future totals
- **Radar modal** — full-screen Windy radar overlay with slide-up animation
- **Air Quality & UV Index** — color-coded gradient bars with descriptive labels
- **Wind compass** — animated SVG needle with speed, direction, and gust data
- **Sunset arc** — animated SVG sun dot with rise/set times
- **Moon phase** — calculated locally, displayed with Unicode emoji and stats
- **Dew point, Humidity, Visibility, Pressure, Feels Like** — each as its own card
- **City search** — powered by the free Open-Meteo Geocoding API (no key required)
- **6 built-in themes** + fully custom theme editor (color pickers, gradient angle)
- **Theme persistence** — saved to localStorage across sessions
- **Auto-refresh** every 10 minutes
- **Responsive** — desktop (1800px+), tablet, and mobile layouts
- **SEO blocked** — `noindex, nofollow` meta tags for private deployments

---

## Tech Stack

| Layer | Technology |
|---|---|
| Frontend | Vanilla HTML5 / CSS3 / JavaScript (no frameworks) |
| Icons | [erikflowers/weather-icons](https://github.com/erikflowers/weather-icons) v2.0.12 |
| Fonts | Google Fonts — Figtree |
| Web server | Nginx (Alpine) |
| Container | Docker / Docker Compose |
| Weather data | [OpenWeatherMap One Call 3.0](https://openweathermap.org/api/one-call-3) |
| Air quality | [OWM Air Pollution API](https://openweathermap.org/api/air-pollution) |
| City search | [Open-Meteo Geocoding API](https://open-meteo.com/en/docs/geocoding-api) (free, no key) |
| Radar | [Windy](https://www.windy.com) embedded iframe |
| Tunneling | Cloudflare Tunnel (optional) |

---

## Prerequisites

- [Docker](https://docs.docker.com/get-docker/) and [Docker Compose](https://docs.docker.com/compose/install/)
- A free [OpenWeatherMap](https://openweathermap.org/api) account with **One Call API 3.0** enabled

---

## Installation

### 1. Clone the repository

```bash
git clone https://github.com/Dennist03/Viewdeck.git
cd Viewdeck
```

### 2. Create your API config

Copy the example config and fill in your own OpenWeatherMap API key:

```bash
cp config.example.js config.js
```

Open `config.js` and replace the placeholder with your key:

```js
const API_KEYS = {
  openweather: 'YOUR_OPENWEATHERMAP_API_KEY_HERE',
};
```

> **Important:** `config.js` is listed in `.gitignore` and will never be committed to the repo. Keep your key private.

### 3. (Optional) Change the default location

Open `index.html` and find the `CONFIG` block near the top of the `<script>` section:

```js
const CONFIG = {
  lat:      28.9006,        // Default: Crystal River, FL
  lon:      -82.5926,
  city:     'Crystal River',
  units:    'imperial',     // 'imperial' (°F / mph) or 'metric' (°C / kph)
  tempUnit: '°F',
  speedUnit: 'mph',
};
```

Update `lat`, `lon`, and `city` to your preferred location. You can also search any city live through the dashboard UI.

### 4. Start the container

```bash
docker compose up -d
```

The dashboard will be available at `http://localhost:5002`.

---

## Docker Compose Reference

```yaml
services:
  weather:
    image: nginx:alpine
    container_name: weather-dash
    restart: unless-stopped
    ports:
      - "5002:80"
    volumes:
      - ./index.html:/usr/share/nginx/html/index.html:ro
      - ./config.js:/usr/share/nginx/html/config.js:ro
      - ./nginx.conf:/etc/nginx/conf.d/default.conf:ro
```

---

## Cloudflare Tunnel (optional)

To expose the dashboard over HTTPS without opening ports, create a Cloudflare Tunnel pointed at `http://localhost:5002`. No other changes to the project are required.

---

## Themes

ViewDeck ships with 6 built-in color presets:

| Name | Description |
|---|---|
| Dark Navy | Deep blue gradient (default) |
| Dark Mode | Near-black with purple tones |
| Slate Blue | Cool slate-blue gradient |
| Forest | Dark green gradient |
| Warm Sand | Warm beige tones |
| White Paper | Light mode — white background, dark text |

Build fully custom themes using the in-app editor (color pickers + gradient angle slider). All custom themes are saved to `localStorage`.

---

## Project Structure

```
Viewdeck/
├── index.html          # Main single-page application
├── config.js           # Your API key (gitignored — never commit)
├── config.example.js   # Safe template to share
├── docker-compose.yml  # Docker service definition
├── nginx.conf          # Nginx web server config
├── .gitignore
├── LICENSE
└── README.md
```

---

## Roadmap

- [ ] Tide chart via [NOAA Tides API](https://tidesandcurrents.noaa.gov/api/) (free, no key)
- [ ] Weather alerts via NWS / OWM One Call
- [ ] Android widget integration
- [ ] ViewDeck dashboard suite expansion

---

## Contributing

Pull requests are welcome. For major changes, please open an issue first to discuss what you would like to change.

---

## License

[MIT](LICENSE) © [Dennist03](https://github.com/Dennist03)
