---
title: Usar un complemento de puerta de enlace personalizado en la extensión de herramienta
description: Desarrollar una extensión de herramienta SDK de Windows Admin Center (proyecto Honolulu) - usar un complemento de puerta de enlace personalizado en la extensión de herramienta
ms.technology: manage
ms.topic: article
author: nwashburn-ms
ms.author: niwashbu
ms.date: 09/18/2018
ms.localizationpriority: medium
ms.prod: windows-server-threshold
ms.openlocfilehash: 4652616478b7b05bde97db48bf84648984b5a325
ms.sourcegitcommit: f1edfc6525e09dd116b106293f9260123a94de0c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/12/2019
ms.locfileid: "9296767"
---
# Usar un complemento de puerta de enlace personalizado en la extensión de herramienta

>Se aplica a: Windows Admin Center, Versión preliminar de Windows Admin Center

En este artículo, usaremos un complemento de puerta de enlace personalizado en una extensión de herramienta nuevo y vacío que hemos creado con la CLI de Windows Admin Center.

## Preparar el entorno ##

Si no lo has hecho ya, sigue las instrucciones de [desarrollar una extensión de herramienta](..\develop-tool.md) para preparar el entorno y crear una extensión de herramienta nuevo y vacío.

## Agregar un módulo al proyecto ##

Si no lo has hecho ya, agrega un nuevo [módulo vacío](add-module.md) al proyecto, que se usarán en el siguiente paso.  

## Agregar integración al complemento de puerta de enlace personalizado ##

Ahora vamos a utilizar un complemento de puerta de enlace personalizado en el módulo nuevo y vacío que acabamos de crear.

### Crear plugin.service.ts

Cambie al directorio del nuevo módulo herramienta creado anteriormente (```\src\app\{!Module-Name}```) y crear un nuevo archivo ```plugin.service.ts```.

Agrega el siguiente código en el archivo que acabas de crear:
``` ts
import { Injectable } from '@angular/core';
import { AppContextService, HttpService } from '@microsoft/windows-admin-center-sdk/angular';
import { Cim, Http, PowerShell, PowerShellSession } from '@microsoft/windows-admin-center-sdk/core';
import { AjaxResponse, Observable } from 'rxjs';

@Injectable()
export class PluginService {
    constructor(private appContextService: AppContextService, private http: Http) {
    }
    
    public getGatewayRestResponse(): Observable<any> {
        let callUrl = this.appContextService.activeConnection.nodeName;

        return this.appContextService.node.get(callUrl, 'features/Sample%20Uno').map(
            (response: any) => {
                return response;
            }
        )
    }
}
```

Cambiar las referencias a ```Sample Uno``` y ```Sample%20Uno``` el nombre de la característica según corresponda.

[!WARNING]
> Es recomendable que integrada en ```this.appContextService.node``` se usa para llamar a cualquier API que se define en el complemento de puerta de enlace personalizado. Esto garantiza si las credenciales son necesarias dentro de tu complemento de puerta de enlace que se controlarán correctamente.

### Modificar module.ts

Abre el ```module.ts``` archivo del nuevo módulo creado anteriormente (es decir, ```{!Module-Name}.module.ts```):

Agrega las siguientes instrucciones de importación:

``` ts
import { HttpService } from '@microsoft/windows-admin-center-sdk/angular';
import { Http } from '@microsoft/windows-admin-center-sdk/core';
import { PluginService } from './plugin.service';
```

Agrega los siguientes proveedores (después de las declaraciones):

``` ts
  ,
  providers: [
    HttpService,
    PluginService,
    Http
  ]
```

### Modificar component.ts

Abre el ```component.ts``` archivo del nuevo módulo creado anteriormente (es decir, ```{!Module-Name}.component.ts```):

Agrega las siguientes instrucciones de importación:

``` ts
import { ActivatedRouteSnapshot } from '@angular/router';
import { AppContextService } from '@microsoft/windows-admin-center-sdk/angular';
import { Subscription } from 'rxjs';
import { Strings } from '../../generated/strings';
import { PluginService } from './plugin.service';
```

Agrega las siguientes variables:

``` ts
  private serviceSubscription: Subscription;
  private responseResult: string;
```

Modificar el constructor y agregar o modificar las siguientes funciones:

``` ts
  constructor(private appContextService: AppContextService, private plugin: PluginService) {
    //
  }

  public ngOnInit() {
    this.responseResult = 'click go to do something';
  }

  public onClick() {
    this.serviceSubscription = this.plugin.getGatewayRestResponse().subscribe(
      (response: any) => {
        this.responseResult = 'response: ' + response.message;
      },
      (error) => {
        console.log(error);
      }
    );
  }
```

### Modificar component.html ###

Abre el ```component.html``` archivo del nuevo módulo creado anteriormente (es decir, ```{!Module-Name}.component.html```):

En el archivo html, agrega el siguiente contenido:
``` html
<button (click)="onClick()" >go</button>
{{ responseResult }}
```

## Compilar y cargar la extensión

Ahora estás listo para [compilar y cargar](..\develop-tool.md#build-and-side-load-your-extension) la extensión en Windows Admin Center.
