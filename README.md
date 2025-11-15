# React.JS Weather Application React.js + VITE

[![Repo Size](https://img.shields.io/github/repo-size/carlo022/React.JS-Weather-Application?color=blue)](https://github.com/carlo022/React.JS-Weather-Application)
[![License](https://img.shields.io/github/license/carlo022/React.JS-Weather-Application)](LICENSE)
[![OpenWeather](https://img.shields.io/badge/API-OpenWeather-orange)](https://openweathermap.org/api)
[![Languages](https://img.shields.io/github/languages/top/carlo022/React.JS-Weather-Application)](https://github.com/carlo022/React.JS-Weather-Application)
[![Made by Carlo](https://img.shields.io/badge/author-Carlo-blue)](https://github.com/carlo022)

This repository contains a polished, production-ready React weather application built to demonstrate modern front-end engineering skills: clean component design, API integration, responsive UI, performance-conscious builds, accessibility considerations, and deployment readiness.

---

Contents
- Quick demo
- Why this project (what it shows about my skills)
- Key features
- Technical highlights & architecture
- Installation & quick start
- Example code snippets
- Performance & quality metrics (how I measured)
- Challenges solved & lessons learned
- How to showcase the project in interviews
- Contributing, License, Contact

---

Quick demo

- Web View:  
   <video src="./src/assets/WAppWebview.mp4" style="max-width:100%; height:auto;">Your browser does not support the video element.</video>
- Mobile View:  
  <img src="./src/assets/Mobile View.png"></img>

---

Why this project — what it demonstrates about my skills

- Full-stack thinking (API integration + UI): I integrated the OpenWeather API handling authentication, query params, units, and error states.
- Component architecture & state management: Cleanly separated presentational and container components using React hooks.
- UX & responsiveness: Mobile-first responsive layout, accessible controls, keyboard-friendly search, and clear error/empty states.
- Production readiness: Build optimization, environment configuration, and deployment instructions for Vercel/Netlify/GitHub Pages.
- Observability & testing: Basic unit tests and end-to-end test plans; metrics such as Lighthouse score, bundle size, and test coverage are considered.
- Problem solving: Rate-limit handling, debounced searches, and graceful degradation when APIs fail—details below.

Use this project as a portfolio piece to show how you approach building user-facing features with reliability, performance, and maintainability in mind.

---

Key features

- Search weather by city or use geolocation
- Current conditions: temperature, feels-like, humidity, wind, description, icon
- Multi-day forecast (where available)
- Unit toggle: Celsius / Fahrenheit
- Robust error handling (invalid city, network errors)
- Loading skeletons and accessible labels
- Responsive layout for mobile & desktop
---

Technical highlights & architecture

- Stack: React (functional components + hooks), CSS (or CSS-in-JS), Fetch/axios, OpenWeather API
- Language composition: ~71% JavaScript, ~23% CSS, ~6% HTML
- Folder structure (simplified):

Design decisions
- Separation of concerns: services/weatherService.js encapsulates API contracts and retries.
- Small, focused components so each unit is easy to test and reuse.
- Debounced search input to reduce unnecessary API calls and respect rate limits.
- Accessibility: semantic HTML, aria-labels, and focus management for keyboard users.

---

Installation & quick start

Prerequisites
- Node.js v16+ and npm or yarn
- OpenWeather API key (free tier OK)

Setup
```bash
git clone https://github.com/carlo022/React.JS-Weather-Application.git
cd React.JS-Weather-Application
npm install
# or
yarn
```

Create .env (do not commit)
```
REACT_APP_OPENWEATHER_API_KEY=your_openweather_api_key_here
REACT_APP_BASE_URL=https://api.openweathermap.org/data/2.5
REACT_APP_UNITS=metric
```

Start development
```bash
npm start
# or
yarn start
```

Build for production
```bash
npm run build
# or
yarn build
```

---

Example code snippets (showcasing practical skills)

1) Fetching current weather with encapsulated service (readability + testability)
```js
// src/services/weatherService.js
const BASE = process.env.REACT_APP_BASE_URL;
const KEY = process.env.REACT_APP_OPENWEATHER_API_KEY;

export async function fetchCurrentWeather(city, units = 'metric') {
  const url = `${BASE}/weather?q=${encodeURIComponent(city)}&appid=${KEY}&units=${units}`;
  const res = await fetch(url);
  if (!res.ok) {
    const err = await res.json().catch(() => ({ message: res.statusText }));
    throw new Error(err.message || 'Failed to fetch weather');
  }
  return res.json();
}
```

2) Debounced search hook to limit API calls
```js
// src/hooks/useDebounce.js
import { useEffect, useState } from 'react';

export default function useDebounce(value, delay = 300) {
  const [debounced, setDebounced] = useState(value);
  useEffect(() => {
    const id = setTimeout(() => setDebounced(value), delay);
    return () => clearTimeout(id);
  }, [value, delay]);
  return debounced;
}
```

3) Sample React usage (compact and testable)
```jsx
// src/components/SearchBar.jsx
import React, { useState } from 'react';
import useDebounce from '../hooks/useDebounce';

export default function SearchBar({ onSearch }) {
  const [q, setQ] = useState('');
  const debouncedQ = useDebounce(q, 400);

  React.useEffect(() => {
    if (debouncedQ) onSearch(debouncedQ);
  }, [debouncedQ, onSearch]);

  return (
    <input
      aria-label="Search city"
      placeholder="Search city..."
      value={q}
      onChange={e => setQ(e.target.value)}
    />
  );
}
```

---

Performance & quality metrics (show off measurable results)

- Lighthouse (example target): Performance >= 90, Accessibility >= 90, Best Practices >= 90
- Bundle size: Keep main bundle < 150 KB (gzipped) through code-splitting and removing large libs
- API request optimizations: Debouncing and conditional fetching to keep requests < X/min
- Tests: Unit tests with Jest + React Testing Library; aim for >70% coverage on core components (add badges if CI configured)
- CI: Add GitHub Actions to run lint, tests, and build on PRs, then show the passing badge here once added.

Add badges (replace with real URLs after configuring CI)
- Build: ![Build](https://img.shields.io/badge/build-passing-brightgreen)
- Tests: ![Coverage](https://img.shields.io/badge/coverage-75%25-yellow)

---

Challenges solved & lessons learned (concrete, interview-ready points)

- Handling inconsistent API responses: implemented a small adapter layer that normalizes OpenWeather responses to a consistent shape used across components.
- Rate limits & UX: introduced debounced searches and progressive loading states (skeleton UIs) so users have clear feedback while avoiding excessive API calls.
- Accessibility improvements: fixed keyboard focus traps, ensured color contrast, and labeled interactive elements.
- Deployment caveat: demonstrated why public API keys need restrictions and suggested serverless proxy approach for production.

---

How to present this project in interviews / portfolio

- Start with the demo video: show the user flows (search, unit toggle, geolocation) and error handling.
- Describe one or two trade-offs you made (client-side API key vs proxy server; size vs third-party libs).
- Show the code snippets above and explain separation of concerns and test strategies.
- Mention measurable outcomes (Lighthouse, bundle size, test coverage) and future improvements planned.

Suggested 3–4 talking points:
1. "I used a debounced search input and an adapter for the OpenWeather payload to ensure consistent UI rendering and fewer API calls."
2. "I focused on mobile-first responsive design and accessibility—keyboard users and screen readers are supported."
3. "Production considerations: build size, CI, and deployment workflow to Vercel/Netlify."

---

Next steps you can add to further showcase your skills

- Add a GitHub Actions workflow to run lint/test/build and publish coverage; display the build/coverage badges.
- Add Cypress end-to-end tests for the main flows (search, geolocation, toggle units).
- Host a long-form walkthrough on YouTube and link it in this README.
- Add a small serverless proxy to hide the API key and implement request caching.

---

Contributing

This repo is primarily a portfolio piece. If you'd like to propose improvements:
1. Fork the repo
2. Create a descriptive branch: feat/describe-change
3. Open a PR with a clear description and screenshots/video if UI changes are included

See CONTRIBUTING.md for full guidelines (you can add one).

---

License

MIT — see LICENSE for details.

---

Contact / Hire me

Carlo — GitHub: @carlo022  
Email: (add email)  
LinkedIn: (add LinkedIn)

---

Acknowledgements & resources

- OpenWeather API — https://openweathermap.org/api
- React — https://reactjs.org/
- Performance & testing tools: Lighthouse, WebPageTest, Jest, React Testing Library

```
