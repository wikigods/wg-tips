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
#### Remover
```js
"@tailwindcss/vite"
"tailwindcss"
```
#### Agregar
```json
"dependencies": {
    "@popperjs/core": "^2.11.8",
    "@vitejs/plugin-vue": "^6.0.7",
    "axios": "^1.18.0",
    "bootstrap": "^5.3.8",
    "concurrently": "^10.0.3",
    "esbuild": "^0.28.1",
    "laravel-vite-plugin": "^3.1.0",
    "sass": "^1.101.0",
    "vite": "^8.0.16",
    "vue": "^3.5.38"
}
```

## Dejar en blanco el archivo `resources/css/app.css`

## En el archivo `vite.config.js`, agregar estas líneas
### v1 
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
### v2
```js
    css: {
    preprocessorOptions: {
        scss: {
            api: 'modern-compiler', // o "modern"
                silenceDeprecations: [
                // 'mixed-decls',
                'color-functions',
                'global-builtin',
                'import',
                'if-function',
            ]
        },
    },
},
build: {
    cssMinify: 'esbuild',
},
```
## Agregar admin-lv
Agregar los siguientes paquetes `package.json`:
```bash
pnpm install @fortawesome/fontawesome-free esbuild fs-extra admin-lv air-datepicker jquery select2 toastr sweetalert2 datatables.net datatables.net-bs5 datatables.net-responsive datatables.net-responsive-bs5 --save-dev
```
Luego ejecutar:

```bash
pnpm run build
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
```php 
return view('welcome'); 
```
a
```php
 return view('admin.index'); 
 ```