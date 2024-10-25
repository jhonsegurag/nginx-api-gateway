Para el correcto funcionamiento de este Nginx dockerizado en nuestro sistema debemos realizar algunos cambios en los archivos con los que ya contamos


**Cambios en el node-ambassador**
- En el archivo index.ts se debe modificar la linea 24 por esto
```
        origin: ['http://localhost:3000', 'http://localhost:4000', 'http://localhost:5000', 'http://localhost']
```
- En el .env se debe cambiar la ruta del checkout por
```
  CHECKOUT_URL=http://localhost/checkout
```

**Cambios en el next-checkout**
- Es necesario crear un archivo con el nombre next.config.js con este contenido
```
module.exports = {
  async rewrites() {
    return [
      {
        source: '/checkout/:code',
        destination: '/:code', // Internally map it to match the expected dynamic page structure
      },
    ];
  },
};
```
- En el archivo constants.ts se debe cambiar el endpoint por
```
    endpoint: 'http://localhost/api/api/checkout',
```
**Cambios en el react-admin**
- En /src/components/Layout.tsx se debe cambiar la linea 28 por
```
        return <Redirect to={'/admin/login'}/>
```
- En /src/components/Menu.tsx se debe cambiar la linea 10 por
  ```
                          <NavLink to={'/admin/users'} className="nav-link" aria-current="page">
  ```
  La linea 15 por
  ```
                         <NavLink to={'/admin/products'} className="nav-link" aria-current="page">
  ```
  Y la linea 20 por
  ```
                          <NavLink to={'/admin/   orders'} className="nav-link" aria-current="page">
  ```
- En /src/components/Nav.tsx se debe cambiar la linea 13 por
```
                <Link to={'/admin/profile'}
```
Y la linea 15 por
```
                <Link to={'/admin/login'} className="p-2 text-white text-decoration-none"
```
- En/src/components/RedirectToUsers.tsx se debe cambiar la linea 4 por
```
export const RedirectToUsers = () => <Redirect to={'/admin/users'}/>;
```
- En /src/pages/Login/tsx se debe cambiar la linea 23 por
```
     return <Redirect to={'/admin'}/>;
```
- En /src/pages/Register.tsx se debe cambiar la linea 33 por
```
            return <Redirect to={'/admin/login'}/>;
```
- En /src/App.tsx desde la linea 18 a la linea 27 se deben realizar los siguientes cambios
```
                <Route path={'/admin'} exact component={RedirectToUsers}/>
                <Route path={'/admin/login'} component={Login}/>
                <Route path={'/admin/register'} component={Register}/>
                <Route path={'/admin/profile'} component={Profile}/>
                <Route path={'/admin/users'} exact component={Users}/>
                <Route path={'/admin/users/:id/links'} component={Links}/>
                <Route path={'/admin/products'} exact component={Products}/>
                <Route path={'/admin/products/create'} component={ProductForm}/>
                <Route path={'/admin/products/:id/edit'} component={ProductForm}/>
                <Route path={'/admin/orders'} exact component={Orders}/>
```
- En index.tsx se debe cambiar la linea 10 por
```
axios.defaults.baseURL = 'http://localhost/api/api/admin/';
```
- En el package.json se debe agregar la linea (se agrego en la linea 5)
```
 "homepage": "/admin",
```

**Cambios en el react-ambassador**

- En /src/pages/Products.tsx se debe cambiar la linea 60 por
```
                message: `Link generated: http://localhost/checkout/${data.code}`
```
- En index.tsx se debe cambiar la linea 10 por
```
axios.defaults.baseURL = 'http://localhost/api/api/ambassador';
```


