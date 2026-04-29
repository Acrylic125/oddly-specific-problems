# Implement a drag-and-drop feature for a Konva element.
## Solution
1. Create the following states and listeners.
```ts
  const [isDragging, setIsDragging] = useState(false);
  const dragStartPosition = useRef<{ x: number; y: number }>({ x: 0, y: 0 });

  const handleDragStart = useCallback(
    (event: Konva.KonvaEventObject<DragEvent>) => {
      dragStartPosition.current = event.target.getPosition();
      setIsDragging(true);
    },
    []
  );

  const handleDragEnd = useCallback(
    (event: Konva.KonvaEventObject<DragEvent>) => {
      setIsDragging(false);
      event.target.position({
        x: dragStartPosition.current.x,
        y: dragStartPosition.current.y,
      });
      event.target.getLayer()?.batchDraw();
    },
    []
  );
```
2. Add the props to the draggable Konva element. 
```tsx
    draggable
    onDragStart={handleDragStart}
    onDragEnd={handleDragEnd}
    onDragCancel={handleDragEnd}
```
