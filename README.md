# React Flow Workshop
For MINT 2024/25 Frontend team :\) 
- Slides: https://docs.google.com/presentation/d/1UmmOxS7EeJK14FCJEuCUc8GvaqoPpr3EruWVu6KaI5I/edit?usp=sharing

## Setting up your own project
1. Scaffold a React-Vite project (or any React project of your choosing) using `npm init vite my-react-flow-app -- --template react`
2. `cd` into root directory and install packages with `npm install`, check it's working with `npm run dev`
3. Install the React Flow components with `npm install @xyflow/react`

## Creating your first flow
1. Add your imports to the top of your `.jsx` file

```js
import { ReactFlow } from '@xyflow/react';
import '@xyflow/react/dist/style.css';
```

2. Create some nodes and edges

```js
const initialNodes = [
  { id: '1', position: { x: 0, y: 0 }, data: { label: '1' } },
  { id: '2', position: { x: 0, y: 100 }, data: { label: '2' } },
];

const initialEdges = [{ id: 'e1-2', source: '1', target: '2' }];
```

3. Make a ReactFlow component and pass along your nodes and edges as attributes. 
- **MAKE SURE IT IS SURROUNDED BY A CONTAINER WITH WIDTH/HEIGHT ATTRIBUTES** (like a `div`, `container`, etc)

```js
export default function App() {
  return (
    <div style={{ width: '100vw', height: '100vh' }}>
      <ReactFlow nodes={initialNodes} edges={initialEdges} />
    </div>
  );
}
```

## Adding Interactivity
1. Some additional imports

```js
import React, { useCallback } from 'react';
import {
  ReactFlow,
  useNodesState,
  useEdgesState,
  addEdge,
} from '@xyflow/react';
```

2. In your page function, initialize Node and Edge states

```js 
  const [nodes, setNodes, onNodesChange] = useNodesState(initialNodes);
  const [edges, setEdges, onEdgesChange] = useEdgesState(initialEdges);
```

3. Define a callback function. In this example, we are making a callback function that will run whenever two nodes are connected.
```js
  const onConnect = useCallback(
    (params) => setEdges((eds) => addEdge(params, eds)),
    [setEdges],
  );
```

4. Modify your ReactFlow component to take in these new states and callback functions.

```js
  return (
    <div style={{ width: '100vw', height: '100vh' }}>
      <ReactFlow
        nodes={nodes}
        edges={edges}
        onNodesChange={onNodesChange}
        onEdgesChange={onEdgesChange}
        onConnect={onConnect}
      />
    </div>
  );
```