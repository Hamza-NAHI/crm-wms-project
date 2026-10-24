# Forma Warehouse Digital Twin

Run `npm install`, then `npm run dev`. Production: `npm run build`. Engine checks: `npm test`.

React + TypeScript + Vite, Three.js/React Three Fiber/Drei, Zustand, Tailwind CSS, Radix-based button composition, Motion, and Lucide.

`src/simulation.ts` owns deterministic event progression, inventory allocation, capacity checks, routing and histories. The 100 ms simulation clock is independent of render frames. `src/WarehouseScene.tsx` renders state and interpolates carton positions. `src/main.tsx` implements operations, inspection, navigation and dashboard metrics.

Try the acceptance workflow: create an order for 5 cartons of SKU-1042, select 10×, choose Orders in the operations feed, and inspect the new order. It progresses through Picking, Packing, Loading, Departing and Dispatched; select its carton IDs to inspect histories. Dispatch is scheduled automatically and can be configured using Manage dispatch.

This is a browser-local simulation, not a production WMS. Reload resets the scenario; speed preference persists. Vehicle and worker motions are simplified and connected to processing state. There is no backend, real inventory integration, or multiplayer synchronization. The exported engine operations provide a separate integration boundary.
