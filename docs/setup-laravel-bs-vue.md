# Guía para empezar un proyecto de Laravel 0 - Bootstrap 5, Vue 3, AdminLTE 5 (Custom)

## Cambiar el archivo `welcome.blade.php`
Puedes ver el archivo de ejemplo original de Laravel en el siguiente enlace:
[Archivo original de `welcome.blade.php`](https://github.com/laravel/laravel/blob/8.x/resources/views/welcome.blade.php)

## Cambiar el archivo `.env.example` y `.env`

| Original                                  | Modificar                                  |
|-------------------------------------------|--------------------------------------------|
| `SESSION_DRIVER=database`                 | `SESSION_DRIVER=file`                      |
| `FILESYSTEM_DISK=local`                   | `FILESYSTEM_DISK=public`                   |
| `QUEUE_CONNECTION=database`               | `QUEUE_CONNECTION=sync`                    |
| `CACHE_STORE=database`                    | `CACHE_STORE=file`                         |

## Instalar [`laravel/ui`](https://packagist.org/packages/laravel/ui) y tener estos cambios presentes

**package.json** (Tener en cuenta que esto puede cambiar)

| Remover             | Agregar                            |
|---------------------|------------------------------------|
| `@tailwindcss/vite` | `"@popperjs/core": "^2.11.8",`     |
| `"tailwindcss"`     | `"@vitejs/plugin-vue": "^6.0.0",`  |
|                     | `"axios": "^1.10.0",`              |
|                     | `"bootstrap": "^5.3.7",`           |
|                     | `"concurrently": "^9.2.0",`        |
|                     | `"laravel-vite-plugin": "^1.3.0",` |
|                     | `"sass": "^1.89.2",`               |
|                     | `"vite": "^6.3.5",`                |
|                     | `"vue": "^3.5.17"`                 |

## Dejar en blanco el archivo `resources/css/app.css`

## En el archivo `vite.config.js`, agregar estas líneas

```js
css: {
  preprocessorOptions: {
    scss: {
      api: 'modern-compiler', // o "modern"
      silenceDeprecations: [
        'mixed-decls',
        'color-functions',
        'global-builtin',
        'import',
      ]
    },
  },
},
```
## Agregar admin-lv
Agregar los siguientes paquetes `package.json`:
```bash
npm install @fortawesome/fontawesome-free @selectize/selectize fs-extra admin-lv flatpickr jquery toastr sweetalert2@~11.3.10 --save-dev
```
Luego ejecutar:

```bash
npm run build
```
luego Descargar los siguientes archivos preparados para 
[`admin-lv`](/wg-tips/admin-lv/admin-lv.zip) copialos en mismo orden que tienen :
En el archivo vite.config.js
agregar estas lineas
```js
'resources/sass/admin/app.scss',
'resources/js/admin/app.js',
```
en archivo routes\web.php
modificar  
```php return view('welcome'); ```
a
```php return view('admin.index'); ```