---
title: Migrar del SDK de centro de administración de Windows 0,1 a 1,0
description: Esta guía le ayudará a migrar desde el SDK del centro de administración de Windows, versión 0,1 a 1,0
ms.technology: manage
ms.topic: article
author: nwashburn-ms
ms.author: niwashbu
ms.date: 02/26/2019
ms.localizationpriority: medium
ms.prod: windows-server
ms.openlocfilehash: c52870178e7caff0abc8ddcccc62966d637dd3c9
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71357058"
---
# <a name="migrate-from-windows-admin-center-sdk-01-to-10"></a>Migrar del SDK de centro de administración de Windows 0,1 a 1,0

>Se aplica a: Versión preliminar de Windows Admin Center

Esta guía le ayudará a migrar desde el SDK del centro de administración de Windows versión 0,1 a 1,0.  

## <a name="1-learn-about-new-controls-with-the-dev-guide-extension"></a>1. Más información sobre los nuevos controles con la extensión de la guía de desarrollo

La versión 1902 y posteriores del centro de administración de Windows incluye la extensión de la **Guía de desarrollo** , que puede usar para buscar ejemplos de controles (incluidos los controles recién disponibles) y escenarios que le ayudarán a crear su propia extensión.  La guía de desarrollo reemplaza a la extensión de **herramientas de desarrollo** de versiones anteriores del SDK.

### <a name="use-the-dev-guide-in-windows-admin-center"></a>Usar la guía de desarrollo en el centro de administración de Windows

La guía de desarrollo está disponible como una solución en la versión 1902 y posteriores del centro de administración de Windows.  Dev Guide está preinstalado, pero debe habilitarse a través de la configuración.

**Habilitar la guía de desarrollo en el centro de administración de Windows:**

* Abra el centro de administración de Windows (versión 1902 y posteriores)
* Haga clic en el icono de **configuración** en la esquina superior derecha de la ventana.
* Seleccione la pestaña **Opciones avanzadas** .
* En *claves de experimento*, haga clic en **Agregar** .
* Escriba un nuevo valor ```msft.sme.shell.devguide``` en el campo vacío creado en el paso anterior
* Haga clic en **Guardar y volver a cargar**

**Abra la guía de desarrollo en el centro de administración de Windows:**

* Abra el centro de administración de Windows (versión 1902 y posteriores)
* Haga clic en la lista desplegable de la parte superior izquierda para mostrar todos los tipos de solución
* Selección de la solución de la **Guía de desarrollo** 
    * Si no ve la solución en la lista, asegúrese de que ha habilitado la guía de desarrollo (consulte la sección anterior) y de que ha recargado el centro de administración de Windows.
* Examine el contenido de la guía de desarrollo seleccionando una de las pestañas
    * **Destino** Contiene ejemplos de código para escenarios de *notificación* y de *Administración*
    * **Permite** Contiene ejemplos de cada control disponible en el SDK
    * **Canal** Contiene ejemplos de funciones de convertidor y de formateador disponibles
    * **Stil** Contiene ejemplos de estilos CSS disponibles en el SDK
    * **MsftSme:** Contiene ejemplos e instrucciones para escenarios avanzados 

### <a name="browse-the-source-code-of-dev-guide-on-github"></a>Examinar el código fuente de la guía de desarrollo en GitHub

Puede examinar el [código fuente](https://github.com/Microsoft/windows-admin-center-sdk/) de la guía de desarrollo en github para encontrar ejemplos de código HTML, CSS y TypeScript de ejemplo.

## <a name="2-prepare-your-development-environment-for-the-latest-sdk"></a>2. Preparación del entorno de desarrollo para el SDK más reciente

Instale o actualice node. js versión [10.15.1 Lts o posterior](https://nodejs.org/en/).

Actualice la CLI del centro de administración de Windows a la versión más reciente:

[//]: # "NPM Uninstall-g windows-admin-center-cli@next"

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

## <a name="3-create-a-new-project-with-the-latest-sdk"></a>3. Creación de un nuevo proyecto con el SDK más reciente

Use la CLI del centro de administración de Windows para crear un nuevo proyecto destinado a la versión ```next``` (SDK 1,0):

[//]: # "WAC Create--Company ' contoso Inc '--Tool ' Manage foo Works '--version experimental"

``` cmd
wac create --company "Contoso Inc" --tool "Manage Foo Works" --version next
```

A continuación, cambie el directorio a la carpeta que acaba de crear y, a continuación, instale las dependencias locales necesarias ejecutando ```npm install ```.

## <a name="4-modify-an-existing-project-to-use-the-latest-sdk"></a>4. Modificar un proyecto existente para usar el SDK más reciente

IMPORTANTE: Realice una copia de seguridad del proyecto antes de continuar.

Modifique la siguiente línea en ```package.json``` para tener como destino la versión ```next``` (SDK 1,0):

[//]: # "' @microsoft/windows-admin-center-sdk ': ' experimental '"

``` json
"@microsoft/windows-admin-center-sdk": "next",
```

A continuación, ejecute ```npm install``` para actualizar las referencias en todo el proyecto.

## <a name="5-use-the-sdk-cli-to-fix-common-migration-issues"></a>5. Uso de la CLI de SDK para solucionar problemas comunes de migración

IMPORTANTE: Realice una copia de seguridad del proyecto antes de continuar.

En la carpeta raíz del proyecto, ejecute el siguiente comando de la CLI en el proyecto para solucionar problemas comunes de migración automáticamente:

``` cmd
wac updateSeven --update
```

Este comando de la CLI soluciona los problemas siguientes automáticamente:

* Regenerar ```package-lock.json```
* Actualizar archivos en el entorno de compilación de angular:
    - ```.gitignore```
    - ```tslint.json```
    - ```tsconfig.json```
    - ```tsconfig.inline.json```
    - ```ng-package.json```
    - ```angular.json```
    - ```src\tsconfig.app.json```
    - ```src\tsconfig.lib.json```
    - ```src\tsconfig.spec.json```

## <a name="6-use-the-sdk-cli-to-understand-common-migration-issues"></a>6. Usar la CLI de SDK para comprender los problemas de migración más comunes

En la carpeta raíz del proyecto, ejecute el siguiente comando de la CLI para auditar el proyecto y buscar problemas comunes de migración que deben solucionarse manualmente:

``` cmd
wac updateSeven --audit
```

Se detectarán las instancias de los siguientes problemas en el proyecto:

### <a name="replace-usage-of-the-following-css-classes-with-these-sme-classes"></a>Reemplace el uso de las siguientes clases CSS con estas clases de SME:

| Clase CSS antigua | Nueva clase CSS |
| -- | -- |
| . auto-Flex-size |  . SME: posición-Flex-auto |
| . Border-todo |  . SME-Border-bajorrelieve-SM y. SME-Border-color-base-90 |
| . Border-inferior |  . SME: Border-Bottom-SM y. SME-Border-Bottom-color-base-90 |
| . Border-horizontal |  . SME: Border-horizontal-SM y. SME-Border-horizontal-color-base-90 |
| . Border-izquierda |  . SME-Border-Left-SM y. SME-Border-Left-color-base-90 |
| . Border-derecha |  . SME: Border-Right-SM y. SME-Border-Right-color-base-90 |
| . Border-superior |  . SME: border-top-SM y. SME-border-top-color-base-90 |
| . Border-vertical |  . SME-Border-vertical-SM y. SME-Border-vertical-color-base-90 |
| . break-Word |  . SME: Arrange-WS-Wrap |
| . BTN |  . SME: botón o botón |
| . BTN: principal |  . SME-Button. SME-Button-Primary o. Button. SME-Button-Primary |
| . color oscuro |  . SME: color-Alt |
| . color claro |  . SME: color: base |
| . color-claro-gris |  . SME: color-base-90 |
| . fixed: tamaño de Flex |  . SME: posición-Flex-ninguno |
| . Flex: diseño |  . SME: Arrange-Stack-h o. SME-Arrange-Stack-v |
| . Font-Bold |  . SME: Font-emphasis1 |
| . Resalte |  . SME: background-color-amarillo |
| . horizontal |  . SME: Arrange-Stack-h |
| . no-desplazar |  . SME: posición-Flex-auto |
| . NoWrap |  . SME: Arrange-Stack-h o. SME-Arrange-Stack-v |
| . relativa |  . SME: diseño-relativo |
| . Relative-Center |  . SME: diseño-Absolute. SME-Position-Center |
| . Reverse |  . SME: Arrange/Stack-invertido |
| . Stretch: absoluto |  . SME-layout-Absolute. SME-Position-bajorrelieve-None |
| . Stretch: fijo |  . SME: diseño-fijo. SME-posición-bajorrelieve-ninguno |
| . Stretch-vertical |  . SME: Position-Stretch-v |
| . Stretch: ancho |  . SME: posición-Stretch-h |
| . vertical |  . SME: Arrange-Stack-v |
| . vertical: solo desplazamiento |  . SME-Arrange-Overflow-Hide-x SME-Arrange-Overflow-auto-y |
| . Wrap |  . SME: Arrange-wrapstack-h o. SME-Arrange-wrapstack-v |

### <a name="replace-usage-of-the-following-components-with-these-sme-components"></a>Reemplace el uso de los siguientes componentes con estos componentes de SME:

| Componente anterior | Nuevo componente |
| -- | -- |
| . alerta |  SME-alerta |
| . alerta: peligro |  SME-alerta |
| . ruta de navegación |  SME-alerta |
| . CheckBox |  SME-Form-Field [type = "CheckBox"] |
| . ComboBox |  SME-Form-Field [type = "Select"] |
| . panel |  SME-layout-Content-Zone-rellenado SME-Arrange-Stack-h |
| . detalles: panel |  SME-Property-Grid |
| . details-panel-contenedor |  SME-Property-Grid |
| . details-Tab |  SME-Property-Grid o SME-Pivot |
| . Details: contenedor |  SME-Property-Grid |
| . deshabilitado |  SME: deshabilitado |
| . Form: botones | SME-formulario-campo |
| . Form-control | SME-formulario-campo |
| . Form: controles | SME-formulario-campo |
| . Form: Grupo | SME-formulario-campo |
| . Form-Group-Label | SME-formulario-campo |
| . Form: entrada | SME-formulario-campo |
| . Form-Stretch | SME-formulario-campo |
| . Input-file | SME-formulario-campo |
| . NAV: pestañas |  SME-dinamización |
| . radio |  SME-Form-Field [type = "radio"] |
| . Required-pista | SME-formulario-campo |
| . control searchbox |  SME-Form-Field [type = "Search"] |
| . toggle-switch |  SME-Form-Field [type = "toggle-switch"] |
| . Tool-Container |  SME-layout-Content-Zone o SME-layout-Content-Zone-acolchado |

### <a name="these-css-classes-are-deprecated-and-are-no-longer-supported"></a>Estas clases CSS están en desuso y ya no se admiten:

| Clase anterior | Desusada |
| -- | -- |
| . aceptable | en desuso |
| . color: error | en desuso |
| . color: información | en desuso |
| . color: correcto | en desuso |
| . color: ADVERTENCIA | en desuso |
| . Delete-Button | en desuso |
| . Details: contenido | en desuso |
| . error: cobertura | en desuso |
| . error: mensaje | en desuso |
| . guided-panel-botón | en desuso |
| . Header-Container | en desuso |
| . icono: Win | en desuso |
| . aplicar sangría | en desuso |
| . no válido | en desuso |
| . Item-List | en desuso |
| . modal: desplazable | en desuso |
| . multi-Section | en desuso |
| . no-Action-bar | en desuso |
| . overflow: márgenes | en desuso |
| . Overflow-Tool | en desuso |
| . progreso: Portada | en desuso |
| . panel derecho | en desuso |
| . Rollup | en desuso |
| . Rollup: estado | en desuso |
| . Rollup: título | en desuso |
| . Rollup: valor | en desuso |
| . control searchbox: barra de acciones | en desuso |
| . Size-h-1 | en desuso |
| . Size-h-2 | en desuso |
| . Size-h-3 | en desuso |
| . Size-h-4 | en desuso |
| . Size-h-Full | en desuso |
| . Size-h-mitad | en desuso |
| . tamaño: v-1 | en desuso |
| . tamaño: v-2 | en desuso |
| . tamaño: v-3 | en desuso |
| . tamaño: v-4 | en desuso |
| . estado: icono | en desuso |
| . SVG: 16px | en desuso |
| . tabla-sangría | en desuso |
| . Table-SM | en desuso |
| . fino | en desuso |
| . icono | en desuso |
| . Tile: cuerpo | en desuso |
| . Tile: contenido | en desuso |
| . Tile-footer | en desuso |
| . Tile: encabezado | en desuso |
| . Tile: diseño | en desuso |
| . Tile-Table | en desuso |
| . Toolbar | en desuso |
| . barra de herramientas | en desuso |
| . Tool-header | en desuso |
| . herramienta-encabezado-cuadro | en desuso |
| . panel de herramientas | en desuso |
| . Usage-bar | en desuso |
| . Usage-área de la barra | en desuso |
| . Usage-barra-fondo | en desuso |
| . Usage-barra-título | en desuso |
| . Usage-bar-Value | en desuso |
| . Usage (gráfico) | en desuso |
| . Usage-Message | en desuso |
| . Usage-Message-Area | en desuso |
| . Usage-Message-title | en desuso |
| . ADVERTENCIA | en desuso |
| . espacio en blanco | en desuso |

## <a name="7-understand-and-resolve-issues-with-observable-objects"></a>7. Comprender y resolver problemas con objetos observables

### <a name="update--rxjs-function-use-for-observable-objects"></a>Actualización del uso de la función ```rxjs``` para objetos observables

Estos son algunos nombres de función comunes que se han cambiado, puede haber otros en el proyecto.

* Actualice ```Observable.empty()``` a ```empty()```
* Actualice ```Observable.of()``` a ```of()```
* Actualice ```.switchMap()``` a ```.pipe(switchMap())```
* Actualice ```.map()``` a ```.pipe(map())```
* Actualice ```flatMap()``` a ```mergeMap()```


### <a name="resolve-runtime-issues-with-map-and-filter-functions-on-observable-objects"></a>Resolver problemas en tiempo de ejecución con funciones ```.map()``` y ```.filter()``` en objetos observables

Si el compilador no puede identificar correctamente el tipo de un objeto ```observable```, las funciones ```.map()``` y ```.filter()``` del objeto ```array``` podrían estar asignadas en su lugar al objeto, lo que provocaría errores en tiempo de ejecución.  Asegúrese de que las funciones devuelven un objeto ```observable``` que especifica un tipo de datos explícito para evitar este problema.

```any``` y ningún tipo de valor devuelto puede producir este problema, busque código con estos patrones:

``` ts
public getMyObservable(): any { //any return type can cause issues
 ...
}

public getMyObservable() { //no return type can cause issues
...
}
```

## <a name="8-resolve-other-common-issues"></a>8. Resolver otros problemas comunes

Estas técnicas le ayudarán a resolver otros problemas comunes:

* Ejecute ```ng lint --fix``` para solucionar problemas comunes de la detección de errores.
* Ejecute ```gulp build``` varias veces para corregir problemas de forma incremental que ```gulp build``` pueda resolver automáticamente.

## <a name="9-build-and-serve-your-project"></a>9. Compilar y servir el proyecto

Ejecute los siguientes comandos para compilar y servir el proyecto con la versión más reciente (SDK 1,0):

``` cmd
gulp build
gulp serve --port 4201
```

## <a name="10-turn-on-dark-theme-in-windows-admin-center"></a>10. Activar el tema oscuro en el centro de administración de Windows

Para activar el tema oscuro en el centro de administración de Windows versión 1902 y versiones posteriores, siga estos pasos:

* Abra el centro de administración de Windows (versión 1902 y posteriores)
* Haga clic en el icono de **configuración** en la esquina superior derecha de la ventana.
* Seleccione la pestaña **Opciones avanzadas** .
* En *claves de experimento*, haga clic en **Agregar** .
* Escriba un nuevo valor ```msft.sme.shell.personalization``` en el campo vacío creado en el paso anterior
* Haga clic en **Guardar y volver a cargar**
* La configuración tendrá ahora una nueva pestaña, **Personalización**.  Seleccionar esta pestaña
* Cambiar los **colores** al **modo oscuro (vista previa)**
