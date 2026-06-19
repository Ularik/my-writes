
```
npx create-next-app@latest my-app --yes
```

### Shadcn
```
npm i shadcn
```

## Tanstack query
```
npm i @tanstack/react-query
npm i @tanstack/react-query-devtools
```

#### providers/providers.tsx
```
'use client';

import {QueryClient, QueryClientProvider} from '@tanstack/react-query';
import {ReactQueryDevtools} from '@tanstack/react-query-devtools';
import {useState} from 'react';

  
export default function Providers({ children }: { children: React.ReactNode }) {
  const [queryClient] = useState(
    () =>
      new QueryClient({
        defaultOptions: {
          queries: {
              retry: 1,
              refetchOnWindowFocus: false,
              staleTime: 1 * 60 * 1000,
              gcTime: 5 * 60 * 1000,
          },
        },
      }),
  );
  
  return (
    <QueryClientProvider client={queryClient}>
        {children}
        <ReactQueryDevtools initialIsOpen={false} />
    </QueryClientProvider>
  );
}
```



#### app/layout.tsx
```
import type { Metadata } from "next";
import { Montserrat } from "next/font/google";
import "./globals.css";
import Providers from "@/providers/provider";

const montserrat = Montserrat({
  variable: "--font-montserrat",
  subsets: ["latin", "cyrillic"],
  weight: ["400", "500", "600", "700"],
});

  
export const metadata: Metadata = {
  title: "Virgin Travel",
  description: "",
  icons: {
    icon: `${process.env.NEXT_BACK_URL}/logo/logo.png`,
  },
};
  

export default function RootLayout({
  children,
}: Readonly<{
  children: React.ReactNode;
}>) {
  return (
    <html className={`${montserrat.variable} h-full antialiased`}>
      <body className="min-h-full flex flex-col">
        <Providers>{children}</Providers>
      </body>
    </html>
  );
}
```