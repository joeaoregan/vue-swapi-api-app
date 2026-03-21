# Vue.js SWAPI (The Star Wars API) Project
## joe-oregan-front-end-dev-app

![Status](https://img.shields.io/website?url=https%3A%2F%2Fvuejs-frontenddemo.onrender.com&logo=render&style=for-the-badge&label=LIVE) 
![Version](https://img.shields.io/badge/version-0.1.0-007ec6?style=for-the-badge)
![Dark Mode](https://img.shields.io/badge/Dark%20Mode-Enabled-black?style=for-the-badge&logo=darkreader&logoColor=white)
![Force](https://img.shields.io/badge/May_The_Force-Be_With_You-red?style=for-the-badge&logo=starwars&logoColor=black)

![Vue 3.5](https://img.shields.io/badge/Vue-3.5.30-35495e?style=for-the-badge&logo=vuedotjs&logoColor=4FC08D)
![Composition API](https://img.shields.io/badge/Vue_3-Composition_API-41b883?style=for-the-badge&logo=vue.js&logoColor=white)
![Router 5](https://img.shields.io/badge/Router-5.0.4-42b883?style=for-the-badge&logo=vue.js&logoColor=white)
![Express 5](https://img.shields.io/badge/Express-5.2.1-000000?style=for-the-badge&logo=express&logoColor=white)
![Axios](https://img.shields.io/badge/Axios-1.13.6-5a29e4?style=for-the-badge&logo=axios&logoColor=white)
![NodeJS](https://img.shields.io/badge/node.js-6DA55F?style=for-the-badge&logo=node.js&logoColor=white)

# ![SWAPI Explorer](https://img.shields.io/badge/SWAPI_EXPLORER-Star_Wars_Database-FFE81F?style=for-the-badge&labelColor=000000)

---

[Vue.js](https://vuejs.org/) project using [SWAPI ~~swapi.dev~~ ~~swapi.tech~~ swapi.dev](https://swapi.dev/)
- Depends which one is working whenever I add updates
- SWAPI, The Star Wars API

## Live Demo

[Render App](https://vuejs-frontenddemo.onrender.com/ "See App on Render")

## Installation & Running

To run the project locally:

```
npm install
npm run serve
```

## Features

- Fetches all [People](https://swapi.dev/api/people/) and [Planet](https://swapi.dev/api/planets/) from SWAPI
- Sortable table columns
- Live search filter
- Clickable homeworld button opens planet popup
- Formatted timestamps (created / edited)
- Star Wars–themed loading screen
- Component‑scoped CSS
- Clean project structure
- Client‑side pagination (page navigation + adjustable page size)

## Screenshots

![Vue.js App 1](https://raw.githubusercontent.com/joeaoregan/vue-front-end-dev-app/master/screenshots/screenshot1.png "Vue.js App 1")

###### Vue.js App 1

![Vue.js App 2](https://raw.githubusercontent.com/joeaoregan/vue-front-end-dev-app/master/screenshots/screenshot2.png "Vue.js App 2 - Popup")

###### Vue.js App 2 - Planet Information Popup

![Vue.js App 3](https://raw.githubusercontent.com/joeaoregan/vue-front-end-dev-app/master/screenshots/screenshot3.png "Vue.js App 3 - User Error")

###### Vue.js App 3 - User Not Found Error

![Vue.js App 4](https://raw.githubusercontent.com/joeaoregan/vue-front-end-dev-app/master/screenshots/screenshot4.png "Vue.js App 4 - Loading Screen")

###### Vue.js App 4 - Star Wars-themed loading screen

### Customize configuration

See [Configuration Reference](https://cli.vuejs.org/config/).

## Tech Stack

- Vue 3 (Composition API + Options API hybrid)
- Axios for API requests
- Component‑scoped CSS
- Hosted on Render

## Project Structure

```
assets/         # Images, icons, logos
components/
  darkmode/     # Dark mode toggle
  loading/      # Loading screen
  navbar/       # Navigation bar
  pagination/   # Pagination component
  popup/        # Planet + Person popups
  scroll/       # Scroll-to-top button
  scrolling/    # Star Wars intro text
  search/       # SearchBox component
  table/        # UserTable component
router/         # Vue Router setup
styles/         # Main style
views/
  About.vue     # Scrolling intro page
  Home.vue      # Main SWAPI table view
  NotFound.vue  # 404 page
App.vue         # Root component
main.js         # App entry point
```

---

## Recent Changes

- Switched back to swapi.dev
- Cleaned up Home.vue and UserTable.vue
- Added Star Wars–themed loading screen
- Moved component styles into dedicated CSS files
- Improved date formatting
- Fixed sorting and filtering logic
- Improved project structure

#### SWAPI.tech

([Swapi.tech](https://swapi.tech)) API data for comparison:
[People](https://swapi.tech/api/people/?page=1&format=json) and [Planet](https://swapi.tech/api/planets/1/?page=1&format=json) JSON data

---

*This project is not affiliated with or endorsed by Lucasfilm Ltd.\
Star Wars and all related properties are trademarks of Lucasfilm Ltd.*