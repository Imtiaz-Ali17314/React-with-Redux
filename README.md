# React with Vanilla Redux

A production-ready demonstration of React 19 integrated with classic **Vanilla Redux** and React Redux for centralized state management. This application, powered by Vite, implements structured global reducer logic, synchronous action dispatching, and manual immutable state management within a responsive interface.

---

## 🛠️ Technology Stack & Dependencies

![React 19](https://img.shields.io/badge/React_19-61DAFB?style=flat-square&logo=react&logoColor=black)
![Redux Core](https://img.shields.io/badge/Redux_Core-764ABC?style=flat-square&logo=redux&logoColor=white)
![Vite](https://img.shields.io/badge/Vite-646CFF?style=flat-square&logo=vite&logoColor=white)
![Bootstrap 5](https://img.shields.io/badge/Bootstrap_5-7952B3?style=flat-square&logo=bootstrap&logoColor=white)
![ESLint](https://img.shields.io/badge/ESLint-4B32C3?style=flat-square&logo=eslint&logoColor=white)

---

## 🚀 Key Features

*   **Classic Centralized Store**: Uses the standard Redux core `createStore` function to initialize a centralized global state container.
*   **Pure Reducer Logic**: Employs a single pure reducer function (`counterReducer`) that evaluates actions using explicit string type matching.
*   **Manual Immutability Management**: Maintains state immutability explicitly using JavaScript spread operators (`{ ...store }`) to copy state parameters during updates, avoiding side-effects.
*   **Explicit Action Dispatching**: Utilizes standard Redux action objects containing plain string `type` values and structured `payload` keys.
*   **Dynamic State Subscriptions**: Subscribes components directly to specific state properties (`store.counter` and `store.privacy`) using React Redux hooks (`useSelector`).
*   **Privacy Shield Overlay**: Conditionally renders UI elements, swapping active reading values with a secure warning string (`Counter is Private !!!!!`) when toggled.
*   **Referenced Value Binding**: Employs React's `useRef` hook to directly reference and reset text input fields, minimizing state re-renders during input collection.
*   **Structured Framework Layout**: Styled with Bootstrap 5 cards, forms, and color-coded buttons.

---

## 📐 Unidirectional Data Flow Architecture

The data flow within the application follows the strict unidirectional Redux pattern using classic plain-object actions and manually copied states:

```mermaid
stateDiagram-v2
    direction TB
    State: Redux Store
    View: React UI
    Dispatch: Action Dispatcher
    Reducer: Pure Reducer Function

    State --> View : useSelector
    View --> Dispatch : Click or Input Event
    Dispatch --> Reducer : Dispatch Plain Action Object
    Reducer --> State : Returns Manually Copied State Object
```

---

## 📂 Repository File Directory

```
React-with-Redux/
├── src/
│   ├── components/            # Presentational & Interactive UI Elements
│   │   ├── Container.jsx      # Card wrapper bounding content layouts
│   │   ├── Controls.jsx       # Event buttons dispatching plain action objects
│   │   ├── DisplayCounter.jsx # Subscribes to and outputs the numerical counter
│   │   ├── Header.jsx         # Primary header presentation
│   │   └── PrivacyMessage.jsx # Fallback component for privacy shield
│   ├── store/                 # State management layer
│   │   └── index.js           # Creates global store, initial state, and reducer function
│   ├── App.css                # Custom application styles
│   ├── App.jsx                # Main controller combining layouts & conditional renders
│   └── main.jsx               # Entrypoint mounting React app and Redux Provider
├── index.html                 # Template landing page
├── package.json               # Package config & dependency versions
├── vite.config.js             # Bundler settings
└── eslint.config.js           # Lint rule definitions
```

---

## 💾 Core Redux Configuration ([store/index.js](file:///d:/for%20CV/My%20learnings/React-with-Redux/src/store/index.js))

The state management configuration is fully consolidated inside the store entrypoint. The reducer function is designed as a pure function that copies existing state elements before modifications:

```javascript
import { createStore } from "redux";

const initialState = {
  counter: 0,
  privacy: false,
};

const counterReducer = (store = initialState, action) => {
  if (action.type === "INCREMENT") {
    return { ...store, counter: store.counter + 1 };
  } else if (action.type === "DECREMENT") {
    return { ...store, counter: store.counter - 1 };
  } else if (action.type === "ADD") {
    return { ...store, counter: store.counter + Number(action.payload.num) };
  } else if (action.type === "SUBTRACT") {
    return { ...store, counter: store.counter - Number(action.payload.num) };
  } else if (action.type === "TOGGLE_PRIVACY") {
    return { ...store, privacy: !store.privacy };
  }
  return store;
};

const counterStore = createStore(counterReducer);

export default counterStore;
```

---

## 💻 UI Interaction Guide

1.  **Direct Increments / Decrements**:
    *   Clicking **`+1`** dispatches `{ type: "INCREMENT" }`.
    *   Clicking **`-1`** dispatches `{ type: "DECREMENT" }`.
2.  **Privacy Lock Toggle**:
    *   Clicking **`Privacy Toggle`** dispatches `{ type: "TOGGLE_PRIVACY" }`.
    *   This switches the `privacy` state boolean, instantly swapping the counter display with the text: `Counter is Private !!!!!`.
3.  **Arbitrary Calculations**:
    *   Type a target integer into the text field.
    *   Click **`Add`** to dispatch `{ type: "ADD", payload: { num: value } }` to dynamically add to the store.
    *   Click **`Subtract`** to dispatch `{ type: "SUBTRACT", payload: { num: value } }` to subtract from the store.
    *   The field value is reset immediately after dispatching.

---

## ⚖️ Classic Redux vs. Redux Toolkit (RTK)

This project showcases the classical configuration of Redux. Key differences compared to Redux Toolkit (RTK) include:

| Architectural Aspect | Classic Redux (This Project) | Redux Toolkit (RTK) |
| :--- | :--- | :--- |
| **Immutability** | Managed manually using standard JS syntax (`return { ...store, value: x }`). State must never be directly mutated. | Handled automatically under-the-hood by **Immer**. Developers can write direct mutations (e.g., `state.value++`). |
| **Boilerplate** | High. Actions, action types, and reducer conditions must be written out explicitly. | Minimal. Slices (`createSlice`) automatically generate matching action creators and action types. |
| **Store Setup** | Uses `createStore`. Middleware and devtool integrations must be manually configured. | Uses `configureStore`. Sets up standard middleware and Redux DevTools extension integrations automatically. |
| **Consolidation** | State and logic are typically placed in separate directories or a single larger file. | Logic is cleanly partitioned into domain-specific slice files containing initial states, actions, and reducers. |

---

## 🚀 Setup & Installation Guidelines

### Prerequisites
Make sure [Node.js](https://nodejs.org/) (version 18.0 or higher) is installed on your local operating system.

### Steps to Run
1.  **Clone the Repository**:
    ```bash
    git clone https://github.com/imtiazaly/React-with-Redux.git
    cd React-with-Redux
    ```
2.  **Install Application Dependencies**:
    ```bash
    npm install
    ```
3.  **Run Development Server**:
    Launch Vite dev server with Hot Module Replacement (HMR):
    ```bash
    npm run dev
    ```
    Access the local application port displayed in the terminal (usually `http://localhost:5173`).
4.  **Produce Production Build**:
    To output optimized static assets inside the `dist` folder:
    ```bash
    npm run build
    ```
5.  **Review Static Bundle**:
    To test the production build locally:
    ```bash
    npm run preview
    ```
