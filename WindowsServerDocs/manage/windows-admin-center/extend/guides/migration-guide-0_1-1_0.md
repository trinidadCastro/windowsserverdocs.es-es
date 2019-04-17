---
title: Migrar desde Windows Admin Center SDK 0,1 a 1,0
description: Esta guía te ayudará a migrar de SDK de Windows Admin Center versión 0,1 a 1,0
ms.technology: manage
ms.topic: article
author: nwashburn-ms
ms.author: niwashbu
ms.date: 02/26/2019
ms.localizationpriority: medium
ms.prod: windows-server-threshold
ms.openlocfilehash: ba0e8cda35c51763b5c10b89c76e2bf07e064dfd
ms.sourcegitcommit: 10d7606a5c2a1ddb234af597f199b6779b0058d4
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/27/2019
ms.locfileid: "9116595"
---
# Migrar desde Windows Admin Center SDK 0,1 a 1,0

>Se aplica a: Windows Admin Center Preview

Esta guía te ayudará a migrar de SDK de Windows Admin Center versión 0,1 a 1,0.  

## 1. obtener información sobre los nuevos controles con la extensión de la Guía de desarrollo

Windows Admin Center 1902 y versiones posteriores incluye la extensión de la **Guía de desarrollo** , que puede usar para encontrar ejemplos de controles (incluidos los controles recién disponibles) y escenarios para ayudarte a crear tu propia extensión.  Guía de desarrollo reemplaza la extensión de **Herramientas de desarrollo** de las versiones anteriores del SDK.

### Usar a la Guía de desarrollo de Windows Admin Center

Guía de desarrollo está disponible como una solución en la versión de Windows Admin Center 1902 y versiones posteriores.  Guía de desarrollo está preinstalado, pero debe estar habilitado mediante la configuración.

**Habilitar la Guía de desarrollo en Windows Admin Center:**

* Abrir Windows Admin Center (versión 1902 y versiones posterior)
* Haz clic en el icono de **configuración** en la esquina superior derecha de la ventana
* Selecciona la pestaña **Opciones avanzadas**
* En *Las claves del experimento*, haz clic en **Agregar**
* Escribe un valor nuevo ```msft.sme.shell.devguide``` en el campo vacío que se creó en el paso anterior
* Haz clic en **Guardar y cargar**

**Abre la Guía de desarrollo en Windows Admin Center:**

* Abrir Windows Admin Center (versión 1902 y versiones posterior)
* Haz clic en el menú desplegable en la parte superior izquierda para mostrar todos los tipos de soluciones
* Seleccione la solución de la **Guía de desarrollo** 
    * Si no ves la solución que aparece, asegúrate de que has habilitado la Guía de desarrollo (consulta la sección anterior) y se vuelve a cargar Windows Admin Center.
* Busca el contenido de la Guía de desarrollo, selecciona una de las pestañas
    * **De inicio:** Contiene ejemplos de código para escenarios de *notificación* y *Administrar como*
    * **Controles:** Contiene ejemplos de cada control disponible en el SDK
    * **Canalizaciones:** Contiene ejemplos de funciones de convertidor y formateador disponibles
    * **Estilos:** Contiene ejemplos de estilos CSS disponibles en el SDK
    * **MsftSme:** Contiene ejemplos e instrucciones para escenarios avanzados 

### Busca el código fuente de la Guía de desarrollo en GitHub

Puede examinar el [código fuente](https://github.com/Microsoft/windows-admin-center-sdk/) de la Guía de desarrollo en GitHub para encontrar HTML, CSS y TypeScript ejemplos de código de ejemplo.

## 2. preparar el entorno de desarrollo para el SDK más reciente

Instalar o actualizar la versión de node.js [10.15.1 LTADOS o una versión posterior](https://nodejs.org/en/).

Actualizar la CLI de Windows Admin Center a la versión más reciente:

[//]: # "NPM desinstalar windows-admin-center-cli@next -g"

``` cmd
npm uninstall -g windows-admin-center-cli
npm install -g windows-admin-center-cli
```

Actualizar las dependencias globales a estas versiones:

``` cmd
npm install npm@6.4.1 -g
npm install @angular/cli@7.1.2 -g
npm install gulp -g
npm install typescript@3.1.6 -g
npm install tslint@5.11.0 -g
```

## 3. crear un nuevo proyecto con el SDK más reciente

Usar la CLI de Windows Admin Center para crear un proyecto nuevo destinados a la ```next``` versión (1.0 SDK):

[//]: # "WAC crea: 'Contoso Inc.': 'Administrar Foo funciona': versión experimental de la herramienta de la compañía"

``` cmd
wac create --company "Contoso Inc" --tool "Manage Foo Works" --version next
```

A continuación, cambia el directorio a la carpeta que acabas de crear y luego instalar necesario de dependencias local mediante la ejecución de ```npm install ```.

## 4. modificar un proyecto existente para usar el SDK más reciente

Importante: Realizar una copia de seguridad del proyecto antes de continuar.

Modificar la siguiente línea en ```package.json``` al destino de la ```next``` versión (1.0 SDK):

[//]: # "'@microsoft/windows-admin-center-sdk': 'experimental'"

``` json
"@microsoft/windows-admin-center-sdk": "next",
```

A continuación, ejecute ```npm install``` para actualizar las referencias en todo el proyecto.

## 5. usar la CLI de SDK para solucionar problemas comunes de migración

Importante: Realizar una copia de seguridad del proyecto antes de continuar.

Desde la carpeta raíz del proyecto, ejecuta el siguiente comando CLI en el proyecto para solucionar problemas comunes de migración de forma automática:

``` cmd
wac updateSeven --update
```

Este comando CLI direcciones automáticamente los siguientes problemas:

* Volver a generar ```package-lock.json```
* Archivos de actualización en el entorno de compilación angular:
    - ```.gitignore```
    - ```tslint.json```
    - ```tsconfig.json```
    - ```tsconfig.inline.json```
    - ```ng-package.json```
    - ```angular.json```
    - ```src\tsconfig.app.json```
    - ```src\tsconfig.lib.json```
    - ```src\tsconfig.spec.json```

## 6. aprovechar la CLI de SDK para comprender los problemas comunes de migración

Desde la carpeta raíz del proyecto, ejecute el siguiente comando CLI auditar el proyecto y encontrar problemas comunes de migración que deben corregirse manualmente:

``` cmd
wac updateSeven --audit
```

Esto encontrar las instancias de los siguientes problemas en el proyecto:

### Reemplaza el uso de las siguientes clases CSS con estas clases de sme:

| Clase CSS antigua | Nueva clase CSS |
| -- | -- |
| tamaño de .auto flexible |  .SME posición flexible automático |
| .Border, todo ello |  .SME-border-bajorrelieve-sm y .sme border color base 90 |
| .Border inferior |  .SME-border-inferior-sm y .sme-border-bottom-color-base-90 |
| .Border horizontal |  .SME-border-horizontal-sm y .sme-border-horizontal-color-base-90 |
| .Border izquierda |  .SME-border-izquierda-sm y .sme-border-left-color-base-90 |
| .Border derecha |  .SME-border-derecha-sm y .sme-border-right-color-base-90 |
| .Border superior |  .SME-border-top-sm y .sme-border-top-color-base-90 |
| .Border vertical |  .SME-border-vertical-sm y .sme-border-vertical-color-base-90 |
| .Break word |  .SME-organizar-ws-wrap |
| .btn |  botón de .sme botón OR |
| .btn principal |  .SME button.sme botón principal o.button.sme-botón-principal |
| .color oscuro |  .SME-color-alt |
| luz .color |  base de color .sme |
| .color-gris claro |  .SME-color-base-90 |
| tamaño de .fixed flexible |  .SME-posición-flexible-ninguno |
| diseño de .flex |  .SME-organizar-pila-h o .sme-organizar-pila-v |
| negrita .font |  .SME-fuente-emphasis1 |
| .Highlight |  .SME en segundo plano color amarillo |
| .horizontal |  .SME-organizar-pila-h |
| desplazamiento de .no |  .SME posición flexible automático |
| .NoWrap |  .SME-organizar-pila-h o .sme-organizar-pila-v |
| .Relative |  relativo de diseño de .sme |
| Centro de .relative |  .SME-diseño-absoluto .sme posición central |
| .Reverse |  .SME organizar-pila-inverso |
| .Stretch absoluto |  .SME diseño-absolutas .sme-posición-bajorrelieve-ninguno |
| .Stretch fijo |  diseño de .sme fija .sme-posición-bajorrelieve-ninguno |
| .Stretch vertical |  .SME-posición-stretch-v |
| ancho de .stretch |  .SME-posición-stretch-h |
| .vertical |  .SME-organizar-pila-v |
| solo .vertical de desplazamiento |  .SME-organizar-desbordamiento-ocultar-x sme-organizar-desbordamiento-auto-y |
| .Wrap |  .SME-organizar-wrapstack-h o .sme-organizar-wrapstack-v |

### Con estos componentes sme, reemplaza el uso de los siguientes componentes:

| Componente antiguo | Nuevo componente |
| -- | -- |
| .Alert |  alerta de SME |
| .Alert peligro |  alerta de SME |
| .breadCrumb |  alerta de SME |
| .CheckBox |  campo del formulario de SME [tipo = "checkbox"] |
| .ComboBox |  campo del formulario de SME [tipo = "seleccionar"] |
| .Dashboard |  SME-diseño-contenido-zona-rellenan sme-organizar-pila-h |
| panel de .details |  cuadrícula de propiedades de SME |
| contenedor de panel .details |  cuadrícula de propiedades de SME |
| pestaña de .details |  cuadrícula de propiedades de SME OR sme-dinámico |
| .Details contenedor |  cuadrícula de propiedades de SME |
| .Disabled |  SME deshabilitada |
| botones de .form | campo del formulario de SME |
| control de .form | campo del formulario de SME |
| controles de .form | campo del formulario de SME |
| grupo de .form | campo del formulario de SME |
| etiqueta de grupo de .form | campo del formulario de SME |
| entrada de .form | campo del formulario de SME |
| .Form stretch | campo del formulario de SME |
| archivo de .input | campo del formulario de SME |
| .NAV pestañas |  SME dinámico |
| .radio |  campo del formulario de SME [tipo = "radio"] |
| pista de .required | campo del formulario de SME |
| .Searchbox |  campo del formulario de SME [tipo = "Buscar"] |
| conmutador de .toggle |  campo del formulario de SME [tipo = "modificador para alternar"] |
| .Tool contenedor |  SME diseño contenido zona o sme-diseño-contenido-zona-rellenan |

### Estas clases CSS están en desuso y ya no son compatibles:

| Clase antigua | En desuso |
| -- | -- |
| .Acceptable | (desusado) |
| error de .color | (desusado) |
| información de .color | (desusado) |
| .color éxito | (desusado) |
| Advertencia de .color | (desusado) |
| botón de .eliminar | (desusado) |
| .Details contenido | (desusado) |
| .Error portada | (desusado) |
| mensaje de .error | (desusado) |
| botón del panel de .guided | (desusado) |
| .Header contenedor | (desusado) |
| .Icon win | (desusado) |
| .Indent | (desusado) |
| .Invalid | (desusado) |
| lista de .item | (desusado) |
| .modal desplazable | (desusado) |
| . con varias secciones | (desusado) |
| barra de acciones .no | (desusado) |
| márgenes de .overflow | (desusado) |
| herramienta de .overflow | (desusado) |
| .Progress portada | (desusado) |
| panel de .right | (desusado) |
| .Rollup | (desusado) |
| estado de .rollup | (desusado) |
| .Rollup título | (desusado) |
| valor de .rollup | (desusado) |
| barra de acciones .searchbox | (desusado) |
| .Size-h-1 | (desusado) |
| .Size-h-2 | (desusado) |
| .Size-h-3 | (desusado) |
| .Size-h-4 | (desusado) |
| .Size-h-completa | (desusado) |
| .Size-h-mitad | (desusado) |
| .Size-v-1 | (desusado) |
| .Size-v-2 | (desusado) |
| .Size-v-3 | (desusado) |
| .Size-v-4 | (desusado) |
| icono de .status | (desusado) |
| .SVG-16 píxeles | (desusado) |
| .Table sangría | (desusado) |
| .Table sm | (desusado) |
| .thin | (desusado) |
| .Tile | (desusado) |
| cuerpo de la .tile | (desusado) |
| .Tile contenido | (desusado) |
| .Tile pie de página | (desusado) |
| encabezado de .tile | (desusado) |
| diseño de .tile | (desusado) |
| tabla de .tile | (desusado) |
| .ToolBar | (desusado) |
| barra de .tool | (desusado) |
| .Tool encabezado | (desusado) |
| cuadro de encabezado .tool | (desusado) |
| panel de .tool | (desusado) |
| barra de .usage | (desusado) |
| área de la barra de .usage | (desusado) |
| fondo de la barra de .usage | (desusado) |
| título de la barra de .usage | (desusado) |
| valor de la barra de .usage | (desusado) |
| gráfico de .usage | (desusado) |
| mensaje de .usage | (desusado) |
| área de mensajes .usage | (desusado) |
| título del mensaje .usage | (desusado) |
| .Warning | (desusado) |
| espacio de .white | (desusado) |

## 7. comprender y resolver problemas con objetos observables

### Actualización ```rxjs``` función uso para objetos observables

Estos son algunos nombres de función común que se han cambiado, puede haber otras personas en el proyecto.

* Actualización ```Observable.empty()``` a ```empty()```
* Actualización ```Observable.of()``` a ```of()```
* Actualización ```.switchMap()``` a ```.pipe(switchMap())```
* Actualización ```.map()``` a ```.pipe(map())```
* Actualización ```flatMap()``` a ```mergeMap()```


### Resolver problemas en tiempo de ejecución con ```.map()``` y ```.filter()``` funciones en objetos observables

Si el compilador no puede identificar correctamente un ```observable``` tipo de objeto, ```.map()``` y ```.filter()``` funciones desde el ```array``` objeto podría asignarse en su lugar en el objeto, lo que produce errores en tiempo de ejecución.  Asegúrate de que las funciones de devuelvan un ```observable``` objeto especificando un tipo de datos explícito para evitar este problema.

```any``` y ningún tipo devuelto puede producir este problema, buscar código con estos patrones:

``` ts
public getMyObservable(): any { //any return type can cause issues
 ...
}

public getMyObservable() { //no return type can cause issues
...
}
```

## 8. solucionar otros problemas comunes

Estas técnicas le ayudarán a solucionar otros problemas comunes:

* Ejecutar ```ng lint --fix``` para solucionar problemas comunes de suciedad
* Ejecutar ```gulp build``` repetidamente incrementalmente solucionar los problemas que ```gulp build``` puede resolver automáticamente

## 9. compila y el proyecto

Ejecute los siguientes comandos para generar y servir para el proyecto con la versión más reciente (1.0 SDK):

``` cmd
gulp build
gulp serve --port 4201
```

## 10. activar el tema oscuro en Windows Admin Center

Para activar el tema oscuro en Windows Admin Center versión 1902 y versiones posteriores, sigue estos pasos:

* Abrir Windows Admin Center (versión 1902 y versiones posterior)
* Haz clic en el icono de **configuración** en la esquina superior derecha de la ventana
* Selecciona la pestaña **Opciones avanzadas**
* En *Las claves del experimento*, haz clic en **Agregar**
* Escribe un valor nuevo ```msft.sme.shell.personalization``` en el campo vacío que se creó en el paso anterior
* Haz clic en **Guardar y cargar**
* Configuración ahora tendrá una nueva pestaña, **personalización**.  Selecciona esta pestaña
* Cambiar los **colores** en **el modo oscuro (versión preliminar)**
