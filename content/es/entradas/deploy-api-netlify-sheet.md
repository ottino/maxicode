---
author: Ottino Maximiliano
title: Publicación de un API en Netlify y Consumo con Google Sheets [Parte 1]
date: 2024-03-11
description: Publicación de un API en Netlify y Consumo con Google Sheets
---

En este artículo, exploraremos cómo publicar un API en Netlify y luego consumirla utilizando Google Sheets. El ejemplo que vamos a desarrollar se centra en un API que proporciona las cuatro cotizaciones del dólar más importantes en Argentina:

- Dólar Libre
- Dólar Banco Nación
- Dólar MEP
- Dólar Tarjeta/Turista


## Fuente de Información y Metodología

La fuente de información para nuestro API es el sitio [Infobae - Dólar Hoy](https://www.infobae.com/economia/divisas/dolar-hoy/). Utilizaremos la técnica de "scraping" para obtener los datos de las cotizaciones del dólar desde este sitio web.

![Infobae Dólar Hoy](/entradas/p1-dolar-infobae.png)

El "scraping" es un proceso automatizado mediante el cual extraemos información de páginas web. Esto nos permite recopilar datos de interés de manera eficiente y utilizarlos en nuestras aplicaciones. En este caso específico, utilizaremos el "scraping" para obtener las cotizaciones del dólar de Infobae y posteriormente exponerlas a través de un API en Netlify.

En la próxima sección, profundizaremos en el proceso de "scraping" y cómo podemos implementarlo para obtener los datos necesarios. Además, aprenderemos cómo configurar y desplegar este API en Netlify para que esté disponible públicamente. ¡Comencemos!

## Api info-dolar.js

Lo primero que hay que hacer es tener el API versionada en GitHub.

El código completo del API en JavaScript se puede encontrar en el siguiente repositorio de GitHub:

- [api-info-dolar](https://github.com/ottino/api-info-dolar)

El API consta principalmente de cuatro funciones, una para obtener cada tipo de dólar, y luego la función handler necesaria para publicarla en Netlify.

Parte del código:

```javascript
export async function handler(event, context) {
    try {
        const dolarLibre = await getDolarLibre();
        const dolarBancoNacion = await getDolarBancoNacion();
        const dolarTurista = await getDolarTurista();
        const dolarMEP = await getDolarMEP();
        
        return {
            statusCode: 200,
            body: JSON.stringify({ dolarLibre, dolarBancoNacion, dolarTurista, dolarMEP })
        };
    } catch (error) {
        console.error('Error al obtener los valores del dólar:', error);
        return {
            statusCode: 500,
            body: JSON.stringify({ error: 'Error al obtener los valores del dólar' })
        };
    }
}
```
### Función Handler y su Importancia

La función handler es esencial en una función de servidor que se despliega en Netlify. En este caso específico, estamos utilizando Netlify Functions, que es una característica que permite ejecutar código del lado del servidor en respuesta a solicitudes HTTP.

Cuando Netlify recibe una solicitud HTTP dirigida a la URL específica de nuestra función, ésta activa la función handler. La función handler es responsable de manejar la solicitud entrante y devolver una respuesta adecuada. En este caso, la función handler se encarga de coordinar las diferentes llamadas a funciones para obtener las diferentes cotizaciones.

### Estructura de carpetas del API

Es crucial prestar atención a la siguiente estructura de carpetas, ya que es fundamental para que Netlify pueda desplegar el API-Functions correctamente:

![Estructura de carpetas](/entradas/p1-estructura-carpetas.png)

- La carpeta `functions` contiene todas las funciones de servidor (en nuestro caso, las funciones API) que queremos desplegar en Netlify. Cada función debe estar contenida en un archivo js separado dentro de subcarpetas con el mismo nombre de la funcion.

- El archivo `info-dolar.js` dentro de `functions\info-dolar` es el archivo que contiene nuestra función API para obtener información sobre las cotizaciones del dólar.

Es importante seguir esta estructura de carpetas para que Netlify pueda detectar y desplegar correctamente nuestras funciones API. En la siguiente sección, veremos cómo configurar y desplegar estas funciones en Netlify.

### Archivo toml

Es importante tener un archivo TOML correctamente configurado para tu proyecto en Netlify. A continuación, te muestro un ejemplo de cómo debería lucir:

```toml
[build.environment]
  NODE_VERSION = "18.16.0"
  
[functions]
  node_bundler = "esbuild"  
```

## Crear cuenta en Netlify y publicar API-Functions

Para comenzar, necesitas crear una cuenta en Netlify y luego publicar tu API-Functions. Sigue estos pasos:

1. Ingresa al siguiente sitio: [Netlify](https://www.netlify.com/)

2. Una vez en la página de inicio de Netlify, busca y haz clic en el botón "Sign Up" (Registrarse) ubicado en la esquina superior derecha de la página.

3. Completa el formulario de registro con tu dirección de correo electrónico y una contraseña segura. También puedes registrarte utilizando tu cuenta de GitHub, GitLab o Bitbucket si lo prefieres.

4. Una vez dentro del panel de control, dirígete a la opción "Sites". Allí, verás la opción "Add new site".

![Añadir nuevo sitio](/entradas/p1-netlify-import.png)

Los siguientes pasos son:

   - Conectarse al proveedor Git.
   - Seleccionar el repositorio.
   - Configurar el sitio y realizar el despliegue.

¡Listo! Ahora tienes una cuenta en Netlify y has conectado tu repositorio, lo que permite un despliegue sincronizado automáticamente.
Cada vez que hagas push sobre tu repositorio automaticament netlify va a realizar un deploy para ver reflejados tus cambios en producción.

![alt text](/entradas/p1-netlify-deploy.png)

Si has seguido todos los pasos hasta aquí, ahora deberías poder acceder a la URL que te ha asignado Netlify y ver lo siguiente:

![Captura de pantalla de la API desplegada en Netlify](/entradas/p1-netlify-api.png)

Esta captura de pantalla muestra la respuesta de la API que hemos desplegado en Netlify. Es un gran hito haber llegado hasta aquí, ya que significa que has configurado correctamente tu API y la has desplegado en un entorno en vivo.

En la próxima parte de esta serie, profundizaremos en cómo consumir esta API desde Google Sheets y crear un flujo automatizado para actualizar los datos de las cotizaciones del dolar.

¡Sigue atento para la parte 2! Si tienes alguna sugerencia o duda, no dudes en contactarme a través de mi correo electrónico: [maxi.ottino@gmail.com](mailto:maxi.ottino@gmail.com).










