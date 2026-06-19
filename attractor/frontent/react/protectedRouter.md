
если пользователь каким-то образом попав на страницу создания товара, при этом будучи незалогиненным, либо залогиненным как обычный пользователь (не администратор), не смог бы увидеть эту страницу. Для этого создадим специальный компонент, которым будем пользоваться в будущем. В файле src/components/ProtectedRoute/ProtectedRoute.tsx:

  
```

import React from 'react';
import { Navigate } from 'react-router-dom';

interface ProtectedRouteProps extends React.PropsWithChildren {
  isAllowed: boolean | null;
}

const ProtectedRoute: React.FC<ProtectedRouteProps> = ({isAllowed, children}) => {

  if (!isAllowed) {
    return <Navigate to="/login"/>;
  }

  return children as React.ReactElement;
};

export default ProtectedRoute;
```
  

И этот компонент мы используем в App.tsx, чтобы "обернуть в них" нужные нам "защищенные" страницы:

  
```
const user = useAppSelector(selectUser);


<Route path="/products/new" element={(
  <ProtectedRoute isAllowed={user && user.role === 'admin'}>
    <NewProduct/>
  </ProtectedRoute>
)}/>
```

Теперь если попытаться зайти на страницу /products/new, мы будем отправлены на логин.