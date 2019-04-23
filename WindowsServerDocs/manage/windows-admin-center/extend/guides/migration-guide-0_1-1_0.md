---
title: Migrar desde Windows Admin Center SDK 0,1 a 1,0
description: Esta guía le ayudará a migrar desde Windows Admin Center SDK versión 0,1 a 1,0
ms.technology: manage
ms.topic: article
author: nwashburn-ms
ms.author: niwashbu
ms.date: 02/26/2019
ms.localizationpriority: medium
ms.prod: windows-server-threshold
ms.openlocfilehash: ba0e8cda35c51763b5c10b89c76e2bf07e064dfd
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59886476"
---
# <a name="migrate-from-windows-admin-center-sdk-01-to-10"></a>Migrar desde Windows Admin Center SDK 0,1 a 1,0

>Se aplica a: Versión preliminar de Windows Admin Center

Esta guía le ayudará a migrar desde Windows Admin Center SDK versión 0,1 a 1,0.  

## <a name="1-learn-about-new-controls-with-the-dev-guide-extension"></a>1. Obtenga información sobre los nuevos controles con la extensión de la Guía de desarrollo

Windows Admin Center 1902 y versiones posteriores incluyen el **Dev Guide** extensión, que puede usar para encontrar ejemplos de controles (incluidos los controles recién disponibles) y escenarios para ayudarle a crear su propia extensión.  Guía de desarrollo reemplaza el **Developer Tools** extensión desde versiones anteriores del SDK.

### <a name="use-the-dev-guide-in-windows-admin-center"></a>Use la Guía de desarrollo en Windows Admin Center

Guía de desarrollo está disponible como una solución en la versión de Windows Admin Center 1902 y versiones posteriores.  Guía de desarrollo está preinstalado, pero debe habilitarse a través de la configuración.

**Habilitar la Guía de desarrollo en Windows Admin Center:**

* Abrir Windows Admin Center (versión 1902 y versiones posterior)
* Haga clic en el **configuración** icono en la esquina superior derecha de la ventana
* Seleccione el **avanzadas** ficha
* En *experimento claves*, haga clic en **agregar**
* Escriba un nuevo valor ```msft.sme.shell.devguide``` en el campo vacío que se creó mediante el paso anterior
* Haga clic en **guardar y volver a cargar**

**Abrir a Guía de desarrollo de Windows Admin Center:**

* Abrir Windows Admin Center (versión 1902 y versiones posterior)
* Haga clic en la lista desplegable en la parte superior izquierda para mostrar todos los tipos de soluciones
* Seleccione el **Dev Guide** solución 
    * Si no ve la solución que aparece, asegúrese de que ha habilitado la Guía de desarrollo (vea la sección anterior) y se vuelve a cargar Windows Admin Center.
* Examine el contenido de la Guía de desarrollo seleccionando una de las pestañas
    * **Inicio:** Contiene ejemplos de código para *administrar como* y *notificación* escenarios
    * **Controles:** Contiene ejemplos de cada control disponible en el SDK
    * **Canalizaciones:** Contiene ejemplos de funciones de formateador y el convertidor de tipos disponibles
    * **Estilos:** Contiene ejemplos de estilos CSS disponibles en el SDK
    * **MsftSme:** Contiene ejemplos e instrucciones para escenarios avanzados 

### <a name="browse-the-source-code-of-dev-guide-on-github"></a>Examinar el código fuente de la Guía de desarrollo en GitHub

Puede examinar el [código fuente](https://github.com/Microsoft/windows-admin-center-sdk/) de guía de desarrollo en GitHub para obtener ejemplos de código TypeScript, CSS y HTML de ejemplo.

## <a name="2-prepare-your-development-environment-for-the-latest-sdk"></a>2. Preparar el entorno de desarrollo para el SDK más reciente

Instalar o actualizar la versión de node.js [10.15.1 LTS o una versión posterior](https://nodejs.org/en/).

Actualización de la CLI de Windows Admin Center a la versión más reciente:

[//]: # "npm uninstall -g windows-admin-center-cli@next"

``` cmd
npm uninstall -g windows-admin-center-cli
npm install -g windows-admin-center-cli
```

Actualice las dependencias globales a estas versiones:

``` cmd
npm install npm@6.4.1 -g
npm install @angular/cli@7.1.2 -g
npm install gulp -g
npm install typescript@3.1.6 -g
npm install tslint@5.11.0 -g
```

## <a name="3-create-a-new-project-with-the-latest-sdk"></a>3. Cree un nuevo proyecto con el SDK más reciente

Use Windows Admin Center CLI para crear un nuevo proyecto destinado a la ```next``` versión (1.0 SDK):

[//]: # "WAC create--"Contoso Inc'--'Administrar Foo Works'--versión experimental de la herramienta de la empresa"

``` cmd
wac create --company "Contoso Inc" --tool "Manage Foo Works" --version next
```

A continuación, cambie el directorio a la carpeta que acaba de crear, a continuación, instale las dependencias necesarias de locales ejecutando ```npm install ```.

## <a name="4-modify-an-existing-project-to-use-the-latest-sdk"></a>4. Modificar un proyecto existente para usar el SDK más reciente

IMPORTANTE: Realizar una copia de seguridad del proyecto antes de continuar.

Modifique la siguiente línea en ```package.json``` al destino la ```next``` versión (1.0 SDK):

[//]: # "'@microsoft/windows-admin-center-sdk': 'experimental'"

``` json
"@microsoft/windows-admin-center-sdk": "next",
```

A continuación, ejecute ```npm install``` para actualizar las referencias a lo largo de su proyecto.

## <a name="5-use-the-sdk-cli-to-fix-common-migration-issues"></a>5. Usar la CLI de SDK para solucionar problemas comunes de migración

IMPORTANTE: Realizar una copia de seguridad del proyecto antes de continuar.

Desde la carpeta raíz del proyecto, ejecute el siguiente comando CLI en el proyecto para corregir automáticamente problemas comunes de migración:

``` cmd
wac updateSeven --update
```

Este comando CLI resuelve automáticamente los problemas siguientes:

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

## <a name="6-use-the-sdk-cli-to-understand-common-migration-issues"></a>6. Use el SDK de CLI para comprender los problemas comunes de migración

Desde la carpeta raíz del proyecto, ejecute el siguiente comando CLI para el proyecto de auditoría y encontrar problemas comunes de migración que deben solucionarse manualmente:

``` cmd
wac updateSeven --audit
```

En el proyecto se buscarán las instancias de los siguientes problemas:

### <a name="replace-usage-of-the-following-css-classes-with-these-sme-classes"></a>Reemplace el uso de las siguientes clases CSS con estas clases sme:

| Clase CSS antigua | Nueva clase CSS |
| -- | -- |
| .auto-flex-size |  .sme-position-flex-auto |
| .border-all |  .SME-border-bajorrelieve-sm y .sme-border-color-base-90 |
| .border-bottom |  .SME-border-inferior-sm y .sme-border-bottom-color-base-90 |
| .border-horizontal |  sm-.sme-border-horizontal y .sme-border-horizontal-color-base-90 |
| .border-left |  sm-.sme-border-izquierda y .sme-border-left-color-base-90 |
| .border-right |  .SME-border-derecha-sm y .sme-border-right-color-base-90 |
| .border-top |  .SME-border-top-sm y .sme-border-top-color-base-90 |
| .border-vertical |  .SME-border-vertical-sm y .sme-border-vertical-color-base-90 |
| .Break word |  .sme-arrange-ws-wrap |
| .btn |  botón de .sme botón OR |
| .btn-primary |  .SME button.sme botón principal OR.button.sme-botón-principal |
| .color-dark |  .sme-color-alt |
| .color-light |  .sme-color-base |
| .color-light-gray |  .sme-color-base-90 |
| .fixed-flex-size |  .sme-position-flex-none |
| .flex-layout |  .SME-organizar-stack-h o .sme-organizar-stack-v |
| .font-bold |  .sme-font-emphasis1 |
| .highlight |  .sme-background-color-yellow |
| .horizontal |  .sme-arrange-stack-h |
| desplazamiento de sujetos a ninguna |  .sme-position-flex-auto |
| .nowrap |  .SME-organizar-stack-h o .sme-organizar-stack-v |
| .Relative |  .sme-layout-relative |
| .relative-center |  Centro absoluto de diseño de .sme .sme de posición |
| .Reverse |  .SME organizar-stack-inverso |
| .stretch-absolute |  .SME-diseño-absoluto .sme-posición-bajorrelieve-ninguno |
| se ha corregido el .stretch |  .SME diseño fijo .sme-posición-bajorrelieve-ninguno |
| .stretch-vertical |  .sme-position-stretch-v |
| .stretch-width |  .SME-posición-stretch-h |
| .vertical |  .sme-arrange-stack-v |
| .vertical-scroll-only |  .SME-organizar-desbordamiento-ocultar-x sme-organizar-overflow-auto-y |
| .wrap |  .SME-organizar-wrapstack-h o .sme-organizar-wrapstack-v |

### <a name="replace-usage-of-the-following-components-with-these-sme-components"></a>Con estos componentes sme, reemplace el uso de los siguientes componentes:

| Componente anterior | Nuevo componente |
| -- | -- |
| .alert |  sme-alert |
| .Alert peligro |  sme-alert |
| .breadCrumb |  sme-alert |
| .checkbox |  sme-form-field[type="checkbox"] |
| .combobox |  sme-form-field[type="select"] |
| .dashboard |  SME-diseño-contenido-zona-rellenan sme-organizar-stack-h |
| .details-panel |  sme-property-grid |
| .details-panel-container |  sme-property-grid |
| .details-tab |  cuadrícula de propiedades de SME OR sme-dinámica |
| .details-wrapper |  sme-property-grid |
| .disabled |  SME deshabilitado |
| .Form botones | sme-form-field |
| .form-control | sme-form-field |
| .form-controls | sme-form-field |
| .form-group | sme-form-field |
| .form-group-label | sme-form-field |
| .form-input | sme-form-field |
| .form-stretch | sme-form-field |
| .input-file | sme-form-field |
| .nav-tabs |  SME dinámica |
| .radio |  sme-form-field[type="radio"] |
| .Required pista | sme-form-field |
| .searchbox |  sme-form-field[type="search"] |
| .Toggle-switch |  sme-form-field[type="toggle-switch"] |
| .tool-container |  SME diseño contenido zona o sme-diseño-contenido-zona-rellena |

### <a name="these-css-classes-are-deprecated-and-are-no-longer-supported"></a>Estas clases CSS están en desuso y ya no se admiten:

| Clase antigua | Desusada |
| -- | -- |
| .Acceptable | (en desuso) |
| .color-error | (en desuso) |
| .color-info | (en desuso) |
| .color-success | (en desuso) |
| .color-warning | (en desuso) |
| .delete-button | (en desuso) |
| .details-content | (en desuso) |
| .Error portada | (en desuso) |
| .error-message | (en desuso) |
| .guided-pane-button | (en desuso) |
| .header-container | (en desuso) |
| .Icon win | (en desuso) |
| .Indent | (en desuso) |
| .Invalid | (en desuso) |
| .item-list | (en desuso) |
| .modal desplazable | (en desuso) |
| . sección múltiples | (en desuso) |
| .no-action-bar | (en desuso) |
| .overflow-margins | (en desuso) |
| herramienta de .overflow | (en desuso) |
| .progress-cover | (en desuso) |
| .right-panel | (en desuso) |
| .Rollup | (en desuso) |
| .rollup-status | (en desuso) |
| .rollup-title | (en desuso) |
| .rollup-value | (en desuso) |
| .searchbox-action-bar | (en desuso) |
| .size-h-1 | (en desuso) |
| .size-h-2 | (en desuso) |
| .size-h-3 | (en desuso) |
| .size-h-4 | (en desuso) |
| .size-h-full | (en desuso) |
| .Size-h-medio | (en desuso) |
| .size-v-1 | (en desuso) |
| .size-v-2 | (en desuso) |
| .size-v-3 | (en desuso) |
| .size-v-4 | (en desuso) |
| .status-icon | (en desuso) |
| .svg-16px | (en desuso) |
| .table-indent | (en desuso) |
| .table-sm | (en desuso) |
| .thin | (en desuso) |
| .Tile | (en desuso) |
| .tile-body | (en desuso) |
| .tile-content | (en desuso) |
| .tile-footer | (en desuso) |
| .tile-header | (en desuso) |
| diseño de .tile | (en desuso) |
| .tile-table | (en desuso) |
| .ToolBar | (en desuso) |
| .tool-bar | (en desuso) |
| .tool-header | (en desuso) |
| .tool-header-box | (en desuso) |
| .tool-pane | (en desuso) |
| .usage-bar | (en desuso) |
| .usage-bar-area | (en desuso) |
| .usage-bar-background | (en desuso) |
| .usage-bar-title | (en desuso) |
| .usage-bar-value | (en desuso) |
| .usage-chart | (en desuso) |
| .usage-message | (en desuso) |
| área de mensajes .usage | (en desuso) |
| .usage-message-title | (en desuso) |
| .warning | (en desuso) |
| .white-space | (en desuso) |

## <a name="7-understand-and-resolve-issues-with-observable-objects"></a>7. Entender y resolver problemas con objetos observables

### <a name="update--rxjs-function-use-for-observable-objects"></a>Actualización ```rxjs``` función se utiliza para objetos observables

Estos son algunos nombres de función común que han cambiado, puede que haya otros usuarios en el proyecto.

* Actualización ```Observable.empty()``` a ```empty()```
* Actualización ```Observable.of()``` a ```of()```
* Actualización ```.switchMap()``` a ```.pipe(switchMap())```
* Actualización ```.map()``` a ```.pipe(map())```
* Actualización ```flatMap()``` a ```mergeMap()```


### <a name="resolve-runtime-issues-with-map-and-filter-functions-on-observable-objects"></a>Resolver problemas en tiempo de ejecución con ```.map()``` y ```.filter()``` funciones con objetos observables

Si el compilador no puede identificar correctamente un ```observable``` tipo de objeto, ```.map()``` y ```.filter()``` funciones desde la ```array``` objeto podría asignarse en su lugar al objeto, lo que produce errores en tiempo de ejecución.  Asegúrese de que las funciones devuelven un ```observable``` objeto que especifica un tipo de datos explícito para evitar este problema.

```any``` y puede producir este problema, busque el código con estos modelos de ningún tipo de valor devuelto:

``` ts
public getMyObservable(): any { //any return type can cause issues
 ...
}

public getMyObservable() { //no return type can cause issues
...
}
```

## <a name="8-resolve-other-common-issues"></a>8. Resolver otros problemas comunes

Estas técnicas que le ayudarán a resolver otros problemas comunes:

* Ejecute ```ng lint --fix``` para solucionar problemas comunes de lint
* Ejecute ```gulp build``` repetidamente incrementalmente corregir problemas que ```gulp build``` puede resolver automáticamente

## <a name="9-build-and-serve-your-project"></a>9. Generar y servir el proyecto

Ejecute los siguientes comandos para generar y servir el proyecto con la versión más reciente (1.0 SDK):

``` cmd
gulp build
gulp serve --port 4201
```

## <a name="10-turn-on-dark-theme-in-windows-admin-center"></a>10. Activar el tema oscuro en Windows Admin Center

Para activar el tema oscuro en Windows Admin Center versión 1902 y versiones posteriores, siga estos pasos:

* Abrir Windows Admin Center (versión 1902 y versiones posterior)
* Haga clic en el **configuración** icono en la esquina superior derecha de la ventana
* Seleccione el **avanzadas** ficha
* En *experimento claves*, haga clic en **agregar**
* Escriba un nuevo valor ```msft.sme.shell.personalization``` en el campo vacío que se creó mediante el paso anterior
* Haga clic en **guardar y volver a cargar**
* Configuración ahora tendrá una nueva pestaña, **personalización**.  Seleccione esta pestaña
* Cambio **colores** a **modo oscuro (versión preliminar)**
