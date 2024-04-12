# vue-calculator

Vue Calculator App
## Overview
The Vue Calculator App is a simple web application built with Vue.js that functions as a basic calculator. It allows users to perform arithmetic operations such as addition, subtraction, multiplication, and division. Additionally, it features a toggle button that enables users to switch between light and dark themes for the user interface.

## Technical Choices
- Vue.js
Vue.js was chosen as the primary framework for building the calculator app due to its simplicity and flexibility. Its component-based architecture made it easy to create reusable UI components for the calculator buttons and display. Vue's reactivity system also simplified the management of the calculator's state, ensuring that the UI updates dynamically in response to user input.

- CSS Variables
CSS variables (custom properties) were utilized for managing the color scheme and theme of the calculator app. By defining CSS variables for background colors, text colors, and button styles, it became straightforward to implement the light and dark themes. This approach also promotes maintainability and allows for easy customization of the app's visual appearance.

- Roboto Font
The Roboto font was chosen for its clean and modern aesthetic, which enhances the readability of text elements in the calculator display. By importing the Roboto font from Google Fonts via a CDN, we ensure consistent typography across different devices and browsers.

## Trade-offs
- UI Complexity
One trade-off made in the development of the Vue Calculator App was the simplicity of the user interface. While the calculator provides essential arithmetic operations, it lacks advanced features such as parentheses for complex expressions, square roots, powers and other scientific operation. This decision was made to prioritize simplicity and ease of use for casual users.

## Future Improvements
Given additional time, several enhancements could be made to the Vue Calculator App:
- Implement support for keyboard input to allow users to perform calculations using their keyboard.
- Enhance the UI with animations and transitions to provide a more polished user experience.
- Enhance the calculator with more operations like square root, raise to power and other scientific operations 

## Getting Started
To run the Vue Calculator App locally, follow these steps:
- Clone the repository: git clone https://github.com/chrixempire/vue-calculator.git
- Navigate to the project directory: cd vue-calculator
- Install dependencies: npm install
- Start the development server: npm run serve
- Open your web browser and navigate to http://localhost:8080




######

This template should help get you started developing with Vue 3 in Vite.

## Recommended IDE Setup

[VSCode](https://code.visualstudio.com/) + [Volar](https://marketplace.visualstudio.com/items?itemName=Vue.volar) (and disable Vetur).

## Customize configuration

See [Vite Configuration Reference](https://vitejs.dev/config/).

## Project Setup

```sh
npm install
```

### Compile and Hot-Reload for Development

```sh
npm run dev
```

### Compile and Minify for Production

```sh
npm run build
```
