---
title: Usar un complemento de puerta de enlace personalizado en la extensión de herramienta
description: Desarrollar una extensión de la herramienta Windows Admin Center SDK (proyecto Honolulu) - usar un complemento de puerta de enlace personalizada en su extensión
ms.technology: manage
ms.topic: article
author: nwashburn-ms
ms.author: niwashbu
ms.date: 09/18/2018
ms.localizationpriority: medium
ms.prod: windows-server-threshold
ms.openlocfilehash: c9b2e9201d58472286b42a9c89a36423f40d143d
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59834516"
---
# <a name="use-a-custom-gateway-plugin-in-your-tool-extension"></a>Usar un complemento de puerta de enlace personalizado en la extensión de herramienta

>Se aplica a: Windows Admin Center, vista previa de Windows Admin Center

En este artículo, usamos un complemento de puerta de enlace personalizada en una extensión de herramienta nueva y vacía que hemos creado con la CLI de Windows Admin Center.

## <a name="prepare-your-environment"></a>Preparar el entorno ##

Si no lo ha hecho ya, siga las instrucciones de [desarrollar una extensión de la herramienta](..\develop-tool.md) para preparar el entorno y crear un nuevo, vacíe la extensión de la herramienta.

## <a name="add-a-module-to-your-project"></a>Agregue un módulo al proyecto ##

Si no lo ha hecho ya, agregue una nueva [módulo vacío](add-module.md) al proyecto, que se usará en el paso siguiente.  

## <a name="add-integration-to-custom-gateway-plugin"></a>Agregar integración al complemento de puerta de enlace personalizada ##

Ahora vamos a usar un complemento de puerta de enlace personalizada en el módulo nuevo y vacío que acabamos de crear.

### <a name="create-pluginservicets"></a>Crear plugin.service.ts

Cambie al directorio del nuevo módulo herramienta creado anteriormente (```\src\app\{!Module-Name}```) y cree un nuevo archivo ```plugin.service.ts```.

Agregue el código siguiente al archivo que acaba de crear:
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

Cambie las referencias a ```Sample Uno``` y ```Sample%20Uno``` a su nombre de característica según corresponda.

### <a name="modify-modulets"></a>Modificar module.ts

Abra el ```module.ts``` archivo del nuevo módulo creado anteriormente (es decir, ```{!Module-Name}.module.ts```):

Agregue las siguientes instrucciones import:

``` ts
import { HttpService } from '@microsoft/windows-admin-center-sdk/angular';
import { Http } from '@microsoft/windows-admin-center-sdk/core';
import { PluginService } from './plugin.service';
```

Agregue los siguientes proveedores (después de declaraciones):

``` ts
  ,
  providers: [
    HttpService,
    PluginService,
    Http
  ]
```

### <a name="modify-componentts"></a>Modificar component.ts

Abra el ```component.ts``` archivo del nuevo módulo creado anteriormente (es decir, ```{!Module-Name}.component.ts```):

Agregue las siguientes instrucciones import:

``` ts
import { ActivatedRouteSnapshot } from '@angular/router';
import { AppContextService } from '@microsoft/windows-admin-center-sdk/angular';
import { Subscription } from 'rxjs';
import { Strings } from '../../generated/strings';
import { PluginService } from './plugin.service';
```

Agregue las siguientes variables:

``` ts
  private serviceSubscription: Subscription;
  private responseResult: string;
```

Modifique el constructor y agregue o modifique las siguientes funciones:

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

### <a name="modify-componenthtml"></a>Modify component.html ###

Abra el ```component.html``` archivo del nuevo módulo creado anteriormente (es decir, ```{!Module-Name}.component.html```):

En el archivo html, agregue el siguiente contenido:
``` html
<button (click)="onClick()" >go</button>
{{ responseResult }}
```

## <a name="build-and-side-load-your-extension"></a>Compilación y cargan la extensión

Ahora está listo para [de compilación y side carga](..\develop-tool.md#build-and-side-load-your-extension) la extensión en Windows Admin Center.
