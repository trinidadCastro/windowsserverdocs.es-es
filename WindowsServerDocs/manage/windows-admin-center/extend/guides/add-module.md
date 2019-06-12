---
title: Agregar un módulo a una extensión de herramienta
description: 'Desarrollar una extensión de la herramienta Windows Admin Center SDK (proyecto Honolulu): adición de un módulo a una extensión de herramienta'
ms.technology: manage
ms.topic: article
author: nwashburn-ms
ms.author: niwashbu
ms.date: 09/18/2018
ms.localizationpriority: medium
ms.prod: windows-server-threshold
ms.openlocfilehash: d8d901097eb280679a388ff66161e3514befcd13
ms.sourcegitcommit: 48bb3e5c179dc520fa879b16c9afe09e07c87629
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/31/2019
ms.locfileid: "66452655"
---
# <a name="add-a-module-to-a-tool-extension"></a>Agregar un módulo a una extensión de herramienta

>Se aplica a: Windows Admin Center, vista previa de Windows Admin Center

En este artículo, agregaremos un módulo vacío a una extensión de herramientas que hemos creado con la CLI de Windows Admin Center.

## <a name="prepare-your-environment"></a>Preparar el entorno

Si no lo ha hecho ya, siga las instrucciones en desarrollar un [herramienta](../develop-tool.md) (o [solución](../develop-solution.md)) extensión para preparar el entorno y crear una extensión de la herramienta nueva y vacía.

## <a name="use-the-angular-cli-to-create-a-module-and-component"></a>Use la CLI de Angular para crear un módulo (y componente)

Si estás familiarizado con Angular, se recomienda leer la documentación en el sitio web de Angular.Io para obtener más información acerca de Angular y NgModule. Para obtener más información acerca de NgModule, haz clic aquí: https://angular.io/guide/ngmodule

* Obtén más información acerca de cómo generar un nuevo módulo en CLI Angular: https://github.com/angular/angular-cli/wiki/generate-module
* Más información acerca de cómo generar un nuevo componente en la CLI de Angular: https://github.com/angular/angular-cli/wiki/generate-component


Abra un símbolo del sistema, cambie el directorio a \src\app en el proyecto y luego ejecute los comandos siguientes, reemplazando ```{!ModuleName}``` con el nombre del módulo (quitados los espacios):

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


## <a name="add-routing-information"></a>Agregar información de enrutamiento

Si no estás familiarizado con Angular, se recomienda encarecidamente obtener información acerca de enrutamiento y navegación en Angular. En las secciones siguientes se definen los elementos de enrutamiento necesarios que permiten a Windows Admin Center navegar a su extensión y entre vistas en su extensión en respuesta a la actividad de usuario. Para obtener más información, haz clic aquí: https://angular.io/guide/router

Use el mismo nombre de módulo que usó en el paso anterior.

### <a name="add-content-to-new-routing-file"></a>Agregar contenido a un nuevo archivo de enrutamiento

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

### <a name="add-content-to-new-module-file"></a>Agregar contenido al nuevo archivo de módulo

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

    | Valor original | Valor nuevo |
    | -------------- | --------- |
    | ```imports: [ CommonModule ]``` | ```imports: [ CommonModule, Routing ]``` |

* Asegúrate de que las instrucciones ```import``` estén ordenadas alfabéticamente por origen.

### <a name="add-content-to-new-component-typescript-file"></a>Agregar contenido al nuevo archivo de typescript de componente

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
### <a name="update-app-routingmodulets"></a>Actualizar aplicación routing.module.ts

Abrir archivo ```app-routing.module.ts```y modifique la ruta de acceso predeterminada para que cargará el nuevo módulo que acaba de crear.  Busque la entrada de ```path: ''```y actualizar ```loadChildren``` para cargar el módulo en lugar del módulo predeterminado:

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
Este es un ejemplo de una ruta de acceso predeterminados actualizados:
``` ts
    {
        path: '', 
        loadChildren: 'app/manage-foo-works-portal/manage-foo-works-portal.module#ManageFooWorksPortalModule'
    },
```


## <a name="build-and-side-load-your-extension"></a>Compilación y cargan la extensión

Ahora ha agregado un módulo a su extensión.  A continuación, puede [de compilación y side carga](../develop-tool.md#build-and-side-load-your-extension) la extensión en Windows Admin Center para ver los resultados.
