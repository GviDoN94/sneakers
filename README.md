# Sneakers

![Vue.js](https://img.shields.io/badge/Vue.js-3.x-brightgreen.svg?logo=vuedotjs) ![Vite](https://img.shields.io/badge/Vite-5.x-brightgreen.svg?logo=vite) ![Vite](https://img.shields.io/badge/Tailwind-blue.svg?logo=tailwindcss) ![Vite](https://img.shields.io/badge/Axios-blue.svg?logo=axios) ![Vite](https://img.shields.io/badge/Lodash-blue.svg?logo=lodash) ![Vite](https://img.shields.io/badge/Prettier-blue.svg?logo=prettier) ![Vite](https://img.shields.io/badge/ESLint-blue.svg?logo=eslint)

**Sneakers Store** is a web application for a sports footwear store. It allows users to browse products, add them to favorites and the shopping cart, and perform searches and sorting before making a purchase.

**You can view it [here](https://sneakers.gvidon94.ru/)**

## Features

- **Product Browsing**: View the catalog of sports footwear.
- **Favorites**: Add products to a favorites list.
- **Shopping Cart**: Easily add products to the shopping cart for later checkout.
- **Search and Sorting**: Quickly search for products by name and sort by various criteria.
- **Checkout**: Complete the purchase with an order form.

## Technologies Used

- **[Vue 3](https://vuejs.org/)** — A framework for building user interfaces.
- **[Vue Router](https://router.vuejs.org/)** — Routing for Vue.js.
- **[Vite](https://vitejs.dev/)** — A fast build tool and development server.
- **[Tailwind CSS](https://tailwindcss.com/)** — A utility-first CSS framework for styling.
- **[Axios](https://axios-http.com/)** — An HTTP client for making API requests.
- **[lodash.debounce](https://lodash.com/docs/4.17.15#debounce)** — A function to limit the rate at which a method is called.
- **[Prettier](https://prettier.io/)** and **[ESLint](https://eslint.org/)** — Tools for code formatting and linting.

## Installation and Setup

### Prerequisites

- Node.js >= 18.x
- npm >= 10.x or yarn >= 1.22.x

### Install Dependencies

```bash
npm install
# or
yarn install
```

### Start the Development Server

```bash
npm run dev
# or
yarn dev

```

### Build the Project for Production

```bash
npm run build
# or
yarn build

```

### Preview the Production Build

```bash
npm run preview
# or
yarn preview

```

### Linting and Formatting

```bash
npm run lint
# or
yarn lint

npm run format
# or
yarn format


```

### Project Structure

- `src`/ — The source code directory.
  - `components/` — Vue components.
  - `pages/` — Specific pages for different parts of the application.
  - `assets/` — Static assets such as images and styles.
- `public/` — Static files that are served from the root.
- `package.json` — Lists dependencies and scripts.
