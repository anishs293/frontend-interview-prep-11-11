In React, lazy loading is a technique that allows components or resources to load only when needed, rather than all at once. This improves the initial load time and optimizes the app’s performance by splitting code and reducing the amount of JavaScript loaded at startup.

Answer:
Lazy loading in React uses React.lazy to defer loading of components until they’re needed. This is often combined with Suspense to show fallback content while the component loads, making the app faster by loading parts of the code only when required.

Key Points:

React.lazy: Loads components on demand, preventing unnecessary code from being bundled initially.
Suspense: Provides a fallback UI while the lazy-loaded component is loading.
Optimizes Performance: Reduces initial load times, especially for larger apps.
Example Code for Lazy Loading a Component
javascript
Copy code
import React, { Suspense } from 'react';

// Lazy load the component
const LazyComponent = React.lazy(() => import('./LazyComponent'));

function App() {
  return (
    <div>
      <h1>My App</h1>
      {/* Suspense provides a fallback UI while LazyComponent is loading */}
      <Suspense fallback={<div>Loading...</div>}>
        <LazyComponent />
      </Suspense>
    </div>
  );
}

export default App;
In this example:

LazyComponent is loaded only when it’s needed.
Suspense shows a fallback UI ("Loading...") until the component finishes loading.