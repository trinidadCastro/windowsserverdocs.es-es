---
title: Usar un complemento de puerta de enlace personalizado en la extensión de herramienta
description: 'Desarrollar una extensión de herramienta SDK del centro de administración de Windows (proyecto Honolulu): usar un complemento de puerta de enlace personalizado en la extensión de la herramienta'
ms.topic: article
author: nwashburn-ms
ms.author: niwashbu
ms.date: 09/18/2018
ms.localizationpriority: medium
ms.openlocfilehash: 739b9e6769d1f2314e73a66d932586863063c7be
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/07/2020
ms.locfileid: "87952690"
---
# <a name="use-a-custom-gateway-plugin-in-your-tool-extension"></a>Usar un complemento de puerta de enlace personalizado en la extensión de herramienta

>Se aplica a: Windows Admin Center, versión preliminar de Windows Admin Center

En este artículo, usaremos un complemento de puerta de enlace personalizado en una nueva extensión de herramienta vacía que hemos creado con la CLI del centro de administración de Windows.

## <a name="prepare-your-environment"></a>Preparación del entorno ##

Si todavía no lo ha hecho, siga las instrucciones de [desarrollo de una extensión de herramienta](../develop-tool.md) para preparar el entorno y crear una nueva extensión de herramienta vacía.

## <a name="add-a-module-to-your-project"></a>Agregar un módulo al proyecto ##

Si aún no lo ha hecho, agregue un nuevo [módulo vacío](add-module.md) al proyecto, que se usará en el paso siguiente.

## <a name="add-integration-to-custom-gateway-plugin"></a>Incorporación de integración al complemento de puerta de enlace personalizada ##

Ahora vamos a usar un complemento de puerta de enlace personalizado en el nuevo módulo vacío que acabamos de crear.

### <a name="create-pluginservicets"></a>Crear plugin. Service. ts

Cambie al directorio del nuevo módulo de herramientas creado anteriormente ( ```\src\app\{!Module-Name}``` ) y cree un archivo nuevo ```plugin.service.ts``` .

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

Cambie las referencias a ```Sample Uno``` y ```Sample%20Uno``` al nombre de la característica según corresponda.

> [!WARNING]
> Se recomienda que el integrado ```this.appContextService.node``` se use para llamar a cualquier API que se defina en el complemento de puerta de enlace personalizada. Esto garantizará que, si se requieren credenciales en el complemento de puerta de enlace, se controlarán correctamente.

### <a name="modify-modulets"></a>Modificar módulo. ts

Abra el ```module.ts``` archivo del nuevo módulo creado anteriormente (es decir, ```{!Module-Name}.module.ts``` ):

Agregue las siguientes instrucciones import:

``` ts
import { HttpService } from '@microsoft/windows-admin-center-sdk/angular';
import { Http } from '@microsoft/windows-admin-center-sdk/core';
import { PluginService } from './plugin.service';
```

Agregue los siguientes proveedores (después de las declaraciones):

``` ts
  ,
  providers: [
    HttpService,
    PluginService,
    Http
  ]
```

### <a name="modify-componentts"></a>Modificar componente. ts

Abra el ```component.ts``` archivo del nuevo módulo creado anteriormente (es decir, ```{!Module-Name}.component.ts``` ):

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

Modifique el constructor y modifique o agregue las siguientes funciones:

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

### <a name="modify-componenthtml"></a>Modificar component.html ###

Abra el ```component.html``` archivo del nuevo módulo creado anteriormente (es decir, ```{!Module-Name}.component.html``` ):

Agregue el siguiente contenido al archivo HTML:
``` html
<button (click)="onClick()" >go</button>
{{ responseResult }}
```

## <a name="build-and-side-load-your-extension"></a>Compilar y cargar la extensión

Ahora está listo para [compilar y cargar](../develop-tool.md#build-and-side-load-your-extension) la extensión en el centro de administración de Windows.
