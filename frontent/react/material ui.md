install
```
npm install @mui/material @emotion/react @emotion/styled
```

main.tsx
```
import { CssBaseline } from '@mui/material';

createRoot(document.getElementById('root')!).render(
  <BrowserRouter>
    <CssBaseline/>
    <App />
  </BrowserRouter>,
)
```