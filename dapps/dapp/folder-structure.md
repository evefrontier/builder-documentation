# Folder Structure

## Folder Structure

After cloning the EVE Frontier Dapps repo, the folder structure is:

```
my-new-eve-dapp/
├─ public/
│  └─ evefrontier.ico
├─ src/
│  ├─ assets/
│  ├─ components/
│  │  ├─ EntityView.tsx
│  │  ├─ Modules.tsx
│  │  └─ SmartAssemblyActions.tsx
│  ├─ App.css
│  ├─ App.tsx
│  ├─ main.tsx
│  ├─ vite-env.d.ts
│  └─ ...
├─ tests/
│  ├─ components/
│  ├─ cypress/
│  │  └─ support/
│  │     ├─ commands.ts
│  │     ├─ component-index.html
│  │     └─ component.ts
│  ├─ e2e/
│  │  └─ specs/
│  │     ├─ connect.spec.ts
│  │     └─ switchnetwork.spec.ts
│  ├─ fixtures/
│  │  └─ example.json
│  ├─ support/
│  │  ├─ commands.ts
│  │  ├─ components.ts
│  │  └─ index.ts
│  └─ unit/
│     ├─ formating.test.ts
│     └─ parser.test.ts
├─ .env.prod
├─ .env.stage
├─ .env.test
├─ .envsample
├─ .gitignore
├─ index.html
├─ package.json
├─ postcss.config.js
├─ README.md
├─ tailwind.config.js
├─ tsconfig.json
├─ tsconfig.node.json
└─ vite.config.ts
```

## Running the DApp

The EVE Smart Assembly Base includes boilerplate logic to connect to the EVE Base Contract data using the World API.

This project uses React Context with exported custom hooks to manage global state. To enable the custom hooks, wrap the app with the `EveWorldProvider` component exported from the `@eveworld/contexts` package.

{% code title="main.tsx" %}
```tsx
import { EveWorldProvider } from "@eveworld/contexts";

  ...

  <EveWorldProvider>
    <ThemeProvider theme={darkTheme}>
      <RouterProvider router={router} />
    </ThemeProvider>
  </EveWorldProvider>,
```
{% endcode %}

Smart objects are accessible using the `useSmartObject` hook.
