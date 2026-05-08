# Fix Blurry Zoom In and Out of React Konva
## Solution
```tsx
import type { Layer as KonvaLayer } from "konva/lib/Layer";

// Capture the state
const [measurements, setMeasurements] = useState<{
  dpr: number;
}>({
  dpr: typeof window !== "undefined" ? window.devicePixelRatio || 1 : 1,
});

// Listen for when the container size changes
useEffect(() => {
  const el = containerRef.current;
  if (!el) return;

  const resizeObserver = new ResizeObserver((cb) => {
    cb.forEach((update) => {
      setMeasurements({
        dpr: window.devicePixelRatio
      });
    });
  });
  resizeObserver.observe(el);
  return () => {
    resizeObserver.disconnect();
  };
}, []);

// React re-render from setMeasurements, you get a frame where the canvas is cleared but React hasn't repainted yet
// So we update AFTER react has repainted.
useEffect(() => {
  layerRef.current?.canvas.setPixelRatio(measurements.dpr);
  layerRef.current?.batchDraw();
}, [measurements.dpr]);

return <div
    className="w-full max-w-7xl overflow-x-hidden h-fit absolute"
    ref={containerRef}
  >
    <Stage
      pixelRatio={pixelRatio}
    >
      <Layer ref={layerRef}>
      </Layer>
    </Stage>
  </div>
```

