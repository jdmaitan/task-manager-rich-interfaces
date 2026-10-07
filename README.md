# Task Manager — Rich Internet Interfaces Final Project

A React + TypeScript task list manager built as the final project for the **Rich Internet Interfaces Development** course in the **Master's in Web Application and Services Development** at the **University of Alicante**.

The application allows authenticated users to create, edit, delete, and organize task lists and tasks. It uses **Firebase Authentication** and **Firebase Realtime Database** as the backend, with a rich frontend powered by **React Router**, **Redux Toolkit**, **Tailwind CSS**, **react-intl**, custom logging, lazy loading, and unit tests.

![React](https://img.shields.io/badge/React-61DAFB?logo=react&logoColor=black)
![TypeScript](https://img.shields.io/badge/TypeScript-3178C6?logo=typescript&logoColor=white)
![Vite](https://img.shields.io/badge/Vite-646CFF?logo=vite&logoColor=white)
![Redux Toolkit](https://img.shields.io/badge/Redux%20Toolkit-764ABC?logo=redux&logoColor=white)
![Firebase](https://img.shields.io/badge/Firebase-FFCA28?logo=firebase&logoColor=black)
![Tailwind CSS](https://img.shields.io/badge/Tailwind%20CSS-38B2AC?logo=tailwind-css&logoColor=white)
![Vitest](https://img.shields.io/badge/Tested%20with-Vitest-6E9F18?logo=vitest&logoColor=white)

## Table of Contents

- [Features](#features)
- [Tech Stack](#tech-stack)
- [Architecture](#architecture)
- [Getting Started](#getting-started)
- [Available Scripts](#available-scripts)
- [Usage](#usage)
- [Routes](#routes)
- [Authentication and Protected Routes](#authentication-and-protected-routes)
- [State Management](#state-management)
- [Firebase Backend](#firebase-backend)
- [Logging and Error Handling](#logging-and-error-handling)
- [Internationalization](#internationalization)
- [Testing](#testing)
- [Academic Context](#academic-context)
- [Author](#author)
- [License](#license)

## Features

- User registration, login, and logout with Firebase Authentication.
- Role management for admin and regular users.
- Global authentication state through `AuthContext`.
- Protected routes for authenticated users.
- Create, edit, and delete task lists with title and description.
- Create, edit, and delete tasks inside a specific task list.
- Toggle task completion status.
- Reusable modal forms with validation.
- Redux Toolkit global state with async thunks.
- Firebase Realtime Database service layer for CRUD operations.
- Lazy loading for pages and conditionally rendered modals.
- Custom logging system with severity levels.
- Redux middleware for logging actions and state changes.
- Robust error handling with user-friendly messages.
- Responsive UI with Tailwind CSS.
- Internationalization with Spanish and English translations.
- Unit tests with Vitest and Testing Library.

## Tech Stack

- **Frontend:** React, TypeScript, Vite
- **Routing:** React Router
- **State Management:** Redux Toolkit, React Redux
- **Backend / BaaS:** Firebase Authentication, Firebase Realtime Database
- **UI Framework:** Tailwind CSS
- **Internationalization:** react-intl, React Context API
- **Testing:** Vitest, jsdom, Testing Library, `@testing-library/jest-dom`
- **Logging:** Custom Logger, Redux middleware

## Architecture

Example project structure based on the final report:

```text
src/
  App.tsx
  main.tsx
  components/
    ProtectedRoute.tsx
    TaskListModal.tsx
    TaskModal.tsx
  contexts/
    AuthContext.tsx
    LanguageContext.tsx
  features/
    taskListsSlice.ts
  pages/
    LandingPage.tsx
    LoginPage.tsx
    RegisterPage.tsx
    TaskListsPage.tsx
    TasksPage.tsx
    NoMatchPage.tsx
  services/
    logging.ts
    FirebaseAuthService.ts
    FirebaseDatabaseUserService.ts
    FirebaseDatabaseTaskService.ts
  store/
    index.ts
    middleware/
      loggerMiddleware.ts
  i18n/
    es.json
    en.json
```

## Getting Started

### Prerequisites

- Node.js
- npm, yarn, or pnpm
- A Firebase project with:
  - Authentication enabled
  - Realtime Database created
  - Email/Password sign-in method enabled

### Installation

```bash
git clone https://github.com/jdmaitan/task-manager-rich-interfaces.git
cd task-manager-rich-interfaces
npm install
```

### Firebase Configuration

Create a Firebase project and enable **Authentication** and **Realtime Database**.

Add your Firebase configuration to the appropriate file or environment variables. For example:

```env
VITE_FIREBASE_API_KEY=your_api_key
VITE_FIREBASE_AUTH_DOMAIN=your_auth_domain
VITE_FIREBASE_DATABASE_URL=your_database_url
VITE_FIREBASE_PROJECT_ID=your_project_id
VITE_FIREBASE_STORAGE_BUCKET=your_storage_bucket
VITE_FIREBASE_MESSAGING_SENDER_ID=your_messaging_sender_id
VITE_FIREBASE_APP_ID=your_app_id
```

Adjust the variable names and setup according to your implementation.

### Run the Development Server

```bash
npm run dev
```

### Build for Production

```bash
npm run build
```

### Preview the Production Build

```bash
npm run preview
```

## Available Scripts

Depending on your `package.json`, the available scripts may include:

```bash
npm run dev       # Start development server
npm run build     # Build for production
npm run preview   # Preview production build
npm run test      # Run unit tests
```

## Usage

1. Open the application.
2. Register a new account or log in with an existing one.
3. Create task lists.
4. Open a task list to manage its tasks.
5. Add, edit, delete, and complete tasks.
6. Switch between Spanish and English.
7. Log out securely.

## Routes

| Route | Page | Access |
| --- | --- | --- |
| `/` | LandingPage | Public |
| `/login` | LoginPage | Public |
| `/register` | RegisterPage | Public |
| `/taskLists` | TaskListsPage | Protected |
| `/taskLists/:taskListId` | TasksPage | Protected |
| `*` | NoMatchPage | Public |

## Authentication and Protected Routes

Authentication is handled by **Firebase Authentication**.

The `AuthContext`:

- Subscribes to Firebase authentication state changes.
- Stores the current user and roles.
- Provides user and role data to the rest of the application.

Protected routes are wrapped with a `ProtectedRoute` component. If an unauthenticated user tries to access `/taskLists` or `/taskLists/:taskListId`, they are redirected to the login page.

The application supports role management for:

- Administrator
- Regular user

## State Management

Global state is managed with **Redux Toolkit**.

The store is configured in `store/index.ts` and includes a `taskLists` slice implemented in `features/taskListsSlice.ts`.

The slice contains:

- `lists`: array of task lists
- `loading`: async operation status
- `error`: error state

Async thunks include:

- `fetchTaskLists`
- `addTaskListAsync`
- `updateTaskListAsync`
- `deleteTaskListAsync`
- `addTaskAsync`
- `updateTaskAsync`
- `deleteTaskAsync`
- `toggleTaskCompletionAsync`

Components use `useSelector` and `useDispatch` to read state and dispatch actions.

## Firebase Backend

Firebase is used as the backend through two main services:

- **Firebase Authentication** for user identity.
- **Firebase Realtime Database** for storing task lists and tasks.

Database service functions include:

- `getTaskListsFromFirebase`
- `addTaskListToFirebase`
- `updateTaskListInFirebase`
- `deleteTaskListFromFirebase`
- `addTaskToFirebase`
- `updateTaskInFirebase`
- `deleteTaskFromFirebase`
- `updateTaskCompletionInFirebase`

## Logging and Error Handling

### Logging

A custom `Logger` class in `services/logging.ts` provides:

- `debug`
- `info`
- `warn`
- `error`

Each log includes a timestamp and severity level. The logger supports a configurable verbosity level.

A Redux middleware in `store/middleware/loggerMiddleware.ts` logs:

- The dispatched action.
- The state before the action.
- The state after the action.

### Error Handling

Async Firebase operations are wrapped in `try...catch` blocks.

Errors are:

1. Captured in the service layer.
2. Logged with detailed information for debugging.
3. Propagated as user-friendly messages.
4. Displayed in the UI using Redux loading and error states.

This avoids exposing technical details to end users while still supporting debugging.

## Internationalization

Internationalization is implemented with **react-intl** and a custom `LanguageContext`.

- The current locale is stored in `localStorage`.
- Spanish is the default language.
- Users can switch between Spanish and English.
- Components use `<FormattedMessage>` with `id` and `defaultMessage`.
- Translation files are stored as `es.json` and `en.json`.

## Testing

Unit tests are configured with **Vitest** and run in a **jsdom** environment.

Testing tools include:

- Vitest
- jsdom
- Testing Library
- `@testing-library/jest-dom`

Tests cover:

- Initial rendering of pages.
- User interactions.
- Navigation between routes.
- Redux state updates.
- Components that depend on authentication, language, and Redux providers.

## Academic Context

This project was developed as the final project for:

**Desarrollo de Interfaces Ricos para Internet (DIRI)**  
Rich Internet Interfaces Development  
Master's in Web Application and Services Development  
University of Alicante  
May 2025

## Author

**José Daniel Maitán Jiménez**

- GitHub: [@jdmaitan](https://github.com/jdmaitan)
- LinkedIn: [jdmaitan](https://linkedin.com/in/jdmaitan)

## License

This project is licensed under the MIT License. See the `LICENSE` file for details.
