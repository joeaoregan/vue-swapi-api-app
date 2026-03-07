# Vue.js SWAPI (The Star Wars API) Project
## joe-oregan-front-end-dev-app

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
components/
  loading/
    LoadingScreen.vue
    style.css
  navbar/
    navbar.vue
    style.css
  popup/
    Popup.vue
    style.css
  scrolling/
    ScrollingText.vue
    style.css
  table/
    style.css
    UserTable.vue
router/
  index.js
styles/
  app.css
About.vue
App.vue
Home.vue
main.js
NotFound.vue
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
