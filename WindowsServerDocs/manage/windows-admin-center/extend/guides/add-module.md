---
title: Agregar un módulo a una extensión de herramienta
description: 'Desarrollar una extensión de herramienta SDK de Windows Admin Center (proyecto Honolulu): agregar un módulo a una extensión de herramienta'
ms.technology: manage
ms.topic: article
author: nwashburn-ms
ms.author: niwashbu
ms.date: 09/18/2018
ms.localizationpriority: medium
ms.prod: windows-server-threshold
ms.openlocfilehash: e6978ce20a7c6da8addb217de8d30f733b40d261
ms.sourcegitcommit: be0144eb59daf3269bebea93cb1c467d67e2d2f1
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/20/2018
ms.locfileid: "4081212"
---
# Agregar un módulo a una extensión de herramienta

>Se aplica a: Windows Admin Center, Versión preliminar de Windows Admin Center

En este artículo, agregaremos un módulo vacío para una extensión de herramienta que hemos creado con la CLI de Windows Admin Center.

## Preparar el entorno

Si no lo has hecho ya, sigue las instrucciones de desarrollar una extensión de [herramienta](..\develop-tool.md) (o [solución](..\develop-solution.md)) para preparar el entorno y crear una extensión de herramienta nuevo y vacío.

## Usar la CLI de Angular para crear un módulo (y el componente)

Si estás familiarizado con Angular, se recomienda leer la documentación en el sitio web de Angular.Io para obtener más información acerca de Angular y NgModule. Para obtener más información acerca de NgModule, haz clic aquí: https://angular.io/guide/ngmodule

* Obtén más información acerca de cómo generar un nuevo módulo en CLI Angular: https://github.com/angular/angular-cli/wiki/generate-module
* Más información acerca de cómo generar un nuevo componente en la CLI de Angular: https://github.com/angular/angular-cli/wiki/generate-component


Abre un símbolo del sistema, cambia el directorio a \src\app en el proyecto y después ejecute los siguientes comandos, sustituyendo ```{!ModuleName}``` con el nombre del módulo (espacios eliminados):

```
cd \src\app
ng generate module {!ModuleName}
ng generate component {!ModuleName}
```

| Valor | Explicación | Ejemplo |
| ----- | ----------- | ------- |
| ```{!ModuleName}``` | El nombre del módulo (espacios eliminados) | ```ManageFooWorksPortal``` |

Ejemplo de uso:
```
cd \src\app
ng generate module ManageFooWorksPortal
ng generate component ManageFooWorksPortal
```


## Agregar información de enrutamiento

Si no estás familiarizado con Angular, se recomienda encarecidamente obtener información acerca de enrutamiento y navegación en Angular. En las secciones siguientes se definen los elementos de enrutamiento necesarios que permiten a Windows Admin Center navegar a su extensión y entre vistas en su extensión en respuesta a la actividad de usuario. Para obtener más información, haz clic aquí: https://angular.io/guide/router

Usar el mismo nombre de módulo utilizado en el paso anterior.

### Agregar contenido a un nuevo archivo de enrutamiento

* Ve a la carpeta del módulo creada con ``` ng generate ``` en el paso anterior.

* Crea un nuevo archivo ```{!module-name}.routing.ts```, siguiendo esta convención de nomenclatura:

    | Valor | Explicación | Ejemplo de nombre de archivo |
    | ----- | ----------- | ------- |
    | ```{!module-name}``` | El nombre del módulo (minúsculas, espacios reemplazados por guiones) | ```manage-foo-works-portal.routing.ts``` |

* Agrega este contenido al archivo que acabas de crear:

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

* Reemplaza los valores en el archivo que acabas de crear por los valores que desees:

    | Valor | Explicación | Ejemplo |
    | ----- | ----------- | ------- |
    | ```{!ModuleName}``` | El nombre del módulo (espacios eliminados) | ```ManageFooWorksPortal``` |
    | ```{!module-name}``` | El nombre del módulo (minúsculas, espacios reemplazados por guiones) | ```manage-foo-works-portal``` |

### Agregar contenido al nuevo archivo de módulo

Abre el archivo ```{!module-name}.module.ts``` encontrado siguiendo esta convención de nomenclatura:

| Valor | Explicación | Ejemplo de nombre de archivo |
| ----- | ----------- | ------- |
| ```{!module-name}``` | El nombre del módulo (minúsculas, espacios reemplazados por guiones) | ```manage-foo-works-portal.module.ts``` |

* Agrega contenido al archivo:

    ``` ts
    import { Routing } from './{!module-name}.routing';
    ```

* Reemplaza los valores en el contenido que acabas de agregar por los valores que desees:

    | Valor | Explicación | Ejemplo |
    | ----- | ----------- | ------- |
    | ```{!module-name}``` | El nombre del módulo (minúsculas, espacios reemplazados por guiones) | ```manage-foo-works-portal``` |

* Modifica la instrucción imports para importar el enrutamiento:

    | Valor original | Nuevo valor |
    | -------------- | --------- |
    | ```imports: [ CommonModule ]``` | ```imports: [ CommonModule, Routing ]``` |

* Asegúrate de que las instrucciones ```import``` estén ordenadas alfabéticamente por origen.

### Agregar contenido al nuevo archivo de typescript de componente

Abre el archivo ```{!module-name}.component.ts``` encontrado siguiendo esta convención de nomenclatura:

| Valor | Explicación | Ejemplo de nombre de archivo |
| ----- | ----------- | ------- |
| ```{!module-name}``` | El nombre del módulo (minúsculas, espacios reemplazados por guiones) | ```manage-foo-works-portal.component.ts``` |
    
Modifica el contenido del archivo a lo siguiente:

``` ts
constructor() {
    // TODO
}

public ngOnInit() {
    // TODO
}
```
### Actualizar la aplicación routing.module.ts

Abre el archivo ```app-routing.module.ts```y modificar la ruta de acceso de forma predeterminada, por lo que se va a cargar el nuevo módulo que acabas de crear.  Busca la entrada de ```path: ''```y actualizar ```loadChildren``` para cargar el módulo en lugar del módulo predeterminado:

| Valor | Explicación | Ejemplo |
| ----- | ----------- | ------- |
| ```{!ModuleName}``` | El nombre del módulo (espacios eliminados) | ```ManageFooWorksPortal``` |
| ```{!module-name}``` | El nombre del módulo (minúsculas, espacios reemplazados por guiones) | ```manage-foo-works-portal``` |

``` ts
    {
        path: '', 
        loadChildren: 'app/{!module-name}/{!module-name}.module#{!ModuleName}Module'
    },
```
Este es un ejemplo de una ruta de acceso predeterminada actualizados:
``` ts
    {
        path: '', 
        loadChildren: 'app/manage-foo-works-portal/manage-foo-works-portal.module#ManageFooWorksPortalModule'
    },
```


## Compilar y cargar la extensión

Ahora has agregado un módulo a la extensión.  A continuación, puedes [compilar y cargar](..\develop-tool.md#build-and-side-load-your-extension) la extensión en el centro de administración de Windows para ver los resultados.
