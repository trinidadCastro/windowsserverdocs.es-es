---
title: Agregar un módulo a una extensión de herramienta
description: 'Desarrollar una extensión de herramienta SDK del centro de administración de Windows (proyecto Honolulu): agregar un módulo a una extensión de herramienta'
ms.topic: article
author: nwashburn-ms
ms.author: niwashbu
ms.date: 09/18/2018
ms.localizationpriority: medium
ms.openlocfilehash: 06331c23730cfdbf1752961f7867b0bebf45cacb
ms.sourcegitcommit: 01b3140f79f5614ce566e8036474feefafbeddc3
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/13/2020
ms.locfileid: "94581419"
---
# <a name="add-a-module-to-a-tool-extension"></a>Agregar un módulo a una extensión de herramienta

>Se aplica a: Windows Admin Center, versión preliminar de Windows Admin Center

En este artículo, se agregará un módulo vacío a una extensión de herramienta que se ha creado con la CLI del centro de administración de Windows.

## <a name="prepare-your-environment"></a>Preparación del entorno

Si todavía no lo ha hecho, siga las instrucciones de desarrollo de una extensión de [herramienta](../develop-tool.md) (o [solución](../develop-solution.md)) para preparar el entorno y crear una nueva extensión de herramienta vacía.

## <a name="use-the-angular-cli-to-create-a-module-and-component"></a>Uso de la CLI de angular para crear un módulo (y componente)

Si no está familiarizado con angular, se recomienda encarecidamente que lea la documentación del sitio web de Angular.Io para obtener información acerca de angular y NgModule. Para obtener más información sobre NgModule, vaya aquí: https://angular.io/guide/ngmodule

* Más información sobre cómo generar un nuevo módulo en la CLI de angular: https://github.com/angular/angular-cli/wiki/generate-module
* Más información sobre la generación de un nuevo componente en la CLI de angular: https://github.com/angular/angular-cli/wiki/generate-component


Abra un símbolo del sistema, cambie el directorio a .\src\app en el proyecto y, a continuación, ejecute los siguientes comandos, reemplazando ```{!ModuleName}``` por el nombre del módulo (espacios quitados):

```
cd .\src\app
ng generate module {!ModuleName}
ng generate component {!ModuleName}
```

| Value | Explicación | Ejemplo |
| ----- | ----------- | ------- |
| ```{!ModuleName}``` | Nombre del módulo (espacios quitados) | ```ManageFooWorksPortal``` |

Ejemplo de uso:
```
cd .\src\app
ng generate module ManageFooWorksPortal
ng generate component ManageFooWorksPortal
```


## <a name="add-routing-information"></a>Agregar información de enrutamiento

Si no está familiarizado con angular, se recomienda encarecidamente obtener información sobre el enrutamiento y la navegación de angular. En las secciones siguientes se definen los elementos de enrutamiento necesarios que permiten al centro de administración de Windows navegar a la extensión y entre las vistas de la extensión en respuesta a la actividad del usuario. Para obtener más información, vaya aquí: https://angular.io/guide/router

Use el mismo nombre de módulo que usó en el paso anterior.

### <a name="add-content-to-new-routing-file"></a>Agregar contenido al nuevo archivo de enrutamiento

* Vaya a la carpeta del módulo que creó  ``` ng generate ``` en el paso anterior.

* Cree un nuevo archivo ```{!module-name}.routing.ts``` , siguiendo esta Convención de nomenclatura:

    | Value | Explicación | Nombre de archivo de ejemplo |
    | ----- | ----------- | ------- |
    | ```{!module-name}``` | Nombre del módulo (en minúsculas, espacios reemplazados por guiones) | ```manage-foo-works-portal.routing.ts``` |

* Agregue este contenido al archivo que acaba de crear:

    ``` ts
    import { NgModule } from '@angular/core';
    import { RouterModule, Routes } from '@angular/router';
    import { {!ModuleName}Component } from './{!module-name}.component';

    const routes: Routes = [
        {
            path: '',
            component: {!ModuleName}Component,
            // if the component has child components that need to be routed to, include them in the children array.
            children: [
                {
                    path: '',
                    redirectTo: 'base',
                    pathMatch: 'full'
                }
            ]
    }];

    @NgModule({
        imports: [
            RouterModule.forChild(routes)
        ],
        exports: [
            RouterModule
        ]
    })
    export class Routing { }
    ```

* Reemplace los valores del archivo que acaba de crear con los valores deseados:

    | Value | Explicación | Ejemplo |
    | ----- | ----------- | ------- |
    | ```{!ModuleName}``` | Nombre del módulo (espacios quitados) | ```ManageFooWorksPortal``` |
    | ```{!module-name}``` | Nombre del módulo (en minúsculas, espacios reemplazados por guiones) | ```manage-foo-works-portal``` |

### <a name="add-content-to-new-module-file"></a>Agregar contenido al nuevo archivo de módulo

Abra ```{!module-name}.module.ts``` el archivo, que se encuentra con la siguiente Convención de nomenclatura:

| Value | Explicación | Nombre de archivo de ejemplo |
| ----- | ----------- | ------- |
| ```{!module-name}``` | Nombre del módulo (en minúsculas, espacios reemplazados por guiones) | ```manage-foo-works-portal.module.ts``` |

* Agregue contenido al archivo:

    ``` ts
    import { Routing } from './{!module-name}.routing';
    ```

* Reemplace los valores en el contenido que acaba de agregar con los valores deseados:

    | Value | Explicación | Ejemplo |
    | ----- | ----------- | ------- |
    | ```{!module-name}``` | Nombre del módulo (en minúsculas, espacios reemplazados por guiones) | ```manage-foo-works-portal``` |

* Modifique la instrucción Imports para importar el enrutamiento:

    | Valor original | Valor nuevo |
    | -------------- | --------- |
    | ```imports: [ CommonModule ]``` | ```imports: [ CommonModule, Routing ]``` |

* Asegúrese de que las ```import``` instrucciones están ordenadas alfabéticamente por origen.

### <a name="add-content-to-new-component-typescript-file"></a>Agregar contenido al nuevo archivo typescript de componente

Abra ```{!module-name}.component.ts``` el archivo, que se encuentra con la siguiente Convención de nomenclatura:

| Value | Explicación | Nombre de archivo de ejemplo |
| ----- | ----------- | ------- |
| ```{!module-name}``` | Nombre del módulo (en minúsculas, espacios reemplazados por guiones) | ```manage-foo-works-portal.component.ts``` |

Modifique el contenido del archivo para lo siguiente:

``` ts
constructor() {
    // TODO
}

public ngOnInit() {
    // TODO
}
```
### <a name="update-app-routingmodulets"></a>Actualizar App-Routing. Module. ts

Abra archivo ```app-routing.module.ts``` y modifique la ruta de acceso predeterminada para que cargue el nuevo módulo que acaba de crear.  Busque la entrada para ```path: ''``` , y actualice  ```loadChildren``` para cargar el módulo en lugar del módulo predeterminado:

| Value | Explicación | Ejemplo |
| ----- | ----------- | ------- |
| ```{!ModuleName}``` | Nombre del módulo (espacios quitados) | ```ManageFooWorksPortal``` |
| ```{!module-name}``` | Nombre del módulo (en minúsculas, espacios reemplazados por guiones) | ```manage-foo-works-portal``` |

``` ts
    {
        path: '',
        loadChildren: 'app/{!module-name}/{!module-name}.module#{!ModuleName}Module'
    },
```
A continuación se muestra un ejemplo de una ruta de acceso predeterminada actualizada:
``` ts
    {
        path: '',
        loadChildren: 'app/manage-foo-works-portal/manage-foo-works-portal.module#ManageFooWorksPortalModule'
    },
```


## <a name="build-and-side-load-your-extension"></a>Compilar y cargar la extensión

Ahora ha agregado un módulo a la extensión.  Después, puede [compilar y cargar](../develop-tool.md#build-and-side-load-your-extension) la extensión en el centro de administración de Windows para ver los resultados.
