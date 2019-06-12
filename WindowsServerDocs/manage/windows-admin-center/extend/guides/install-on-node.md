---
title: Desarrollar una extensión de herramienta
description: Desarrollar una extensión de la herramienta Windows Admin Center SDK (proyecto Honolulu)
ms.technology: manage
ms.topic: article
author: nwashburn-ms
ms.author: niwashbu
ms.date: 09/18/2018
ms.localizationpriority: medium
ms.prod: windows-server-threshold
ms.openlocfilehash: 1a068c0d33887e8e9287ff15c1aa14f3dc84915a
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/31/2019
ms.locfileid: "66445932"
---
# <a name="install-extension-payload-on-a-managed-node"></a>Instalar la carga de la extensión en un nodo administrado

>Se aplica a: Windows Admin Center, vista previa de Windows Admin Center

## <a name="setup"></a>Programa de instalación
> [!NOTE]
> Para seguir esta guía, necesita generar 1.2.1904.02001 o superior. Para comprobar la compilación de número abrir Windows Admin Center y haga clic en el signo de interrogación en la esquina superior derecha.

Si no lo ha hecho ya, cree un [herramienta extensión](../develop-tool.md) para Windows Admin Center. Después de haber completado esta marca nota de los valores utilizados al crear una extensión:

| Valor | Explicación | Ejemplo |
| ----- | ----------- | ------- |
| ```{!Company Name}``` | Nombre de su compañía (con espacios) | ```Contoso``` |
| ```{!Tool Name}``` | El nombre de la herramienta (con espacios) | ```InstallOnNode``` |

Dentro de la carpeta de extensión de la herramienta crea un ```Node``` carpeta (```{!Tool Name}\Node```). Nada se colocan en esta carpeta se copiarán al nodo administrado al usar esta API. Agregar los archivos necesarios para el caso de uso. 

Cree también un ```{!Tool Name}\Node\installNode.ps1``` secuencia de comandos. Esta secuencia de comandos se ejecutan en el nodo administrado una vez que todos los archivos se copian desde el ```{!Tool Name}\Node``` carpeta al nodo administrado. Agregar ninguna lógica adicional para el caso de uso. Un ejemplo ```{!Tool Name}\Node\installNode.ps1``` archivo:

``` ps1
# Add logic for installing payload on managed node
echo 'Success'
```

> [!NOTE]
> ```{!Tool Name}\Node\installNode.ps1``` tiene un nombre específico que se va a buscar la API. Cambiar el nombre de este archivo se producirá un error.


## <a name="integration-with-ui"></a>Integración con la interfaz de usuario

Actualización ```\src\app\default.component.ts``` al siguiente:

``` ts
import { Component } from '@angular/core';
import { AppContextService } from '@microsoft/windows-admin-center-sdk/angular';
import { Observable } from 'rxjs';

@Component({
  selector: 'default-component',
  templateUrl: './default.component.html',
  styleUrls: ['./default.component.css']
})

export class DefaultComponent {
  constructor(private appContextService: AppContextService) { }

  public response: any;
  public loading = false;

  public installOnNode() {
    this.loading = true;
    this.post('{!Company Name}.{!Tool Name}', '1.0.0',
      this.appContextService.activeConnection.nodeName).subscribe(
        (response: any) => {
          console.log(response);
          this.response = response;
          this.loading = false;
        },
        (error) => {
          console.log(error);
          this.response = error;
          this.loading = false;
        }
      );
  }

  public post(id: string, version: string, targetNode: string): Observable<any> {
    return this.appContextService.node.post(targetNode,
      `features/extensions/${id}/versions/${version}/install`);
  }

}
```
Actualice los marcadores de posición para valores que se usaron al crear la extensión:
``` ts
this.post('contoso.install-on-node', '1.0.0',
      this.appContextService.activeConnection.nodeName).subscribe(
        (response: any) => {
          console.log(response);
          this.response = response;
          this.loading = false;
        },
        (error) => {
          console.log(error);
          this.response = error;
          this.loading = false;
        }
      );
```

También actualizar ```\src\app\default.component.html``` para:
``` html
<button (click)="installOnNode()">Click to install</button>
<sme-loading-wheel *ngIf="loading" size="large"></sme-loading-wheel>
<p *ngIf="response">{{response}}</p>
```
Por último, ```\src\app\default.module.ts```:
``` ts
import { CommonModule } from '@angular/common';
import { NgModule } from '@angular/core';

import { LoadingWheelModule } from '@microsoft/windows-admin-center-sdk/angular';
import { DefaultComponent } from './default.component';
import { Routing } from './default.routing';

@NgModule({
  imports: [
    CommonModule,
    LoadingWheelModule,
    Routing
  ],
  declarations: [DefaultComponent]
})
export class DefaultModule { }

```

## <a name="creating-and-installing-a-nuget-package"></a>Crear e instalar un paquete de NuGet

El último paso es crear un paquete de NuGet con los archivos que hemos agregado y, a continuación, instalar ese paquete en Windows Admin Center.

Siga el [extensiones publicación](../publish-extensions.md) guía si no ha creado un paquete de extensión antes. 
> [!IMPORTANT]
> En el archivo .nuspec para esta extensión, es importante que la ```<id>``` valor coincide con el nombre del proyecto ```manifest.json``` y ```<version>``` coincide con lo que se agregó a ```\src\app\default.component.ts```. Agregue también una entrada en ```<files>```: 
> 
> ```<file src="Node\**\*.*" target="Node" />```.

``` xml
<?xml version="1.0" encoding="utf-8"?>
<package xmlns="http://schemas.microsoft.com/packaging/2011/08/nuspec.xsd">
  <metadata>
    <id>contoso.install-on-node</id>
    <version>1.0.0</version>
    <authors>Contoso</authors>
    <owners>Contoso</owners>
    <requireLicenseAcceptance>false</requireLicenseAcceptance>
    <projectUrl>https://msft-sme.myget.org/feed/windows-admin-center-feed/package/nuget/contoso.sme.install-on-node-extension</projectUrl>
    <licenseUrl>http://YourLicenseLink</licenseUrl>
    <iconUrl>http://YourLogoLink</iconUrl>
    <description>Install on node extension by Contoso</description>
    <copyright>(c) Contoso. All rights reserved.</copyright> 
  </metadata>
    <files>
    <file src="bundle\**\*.*" target="ux" />
    <file src="package\**\*.*" target="gateway" />
    <file src="Node\**\*.*" target="Node" />
  </files>
</package>
```

Una vez creado este paquete, agregue una ruta de acceso a esa fuente. En Windows Admin Center vaya a Configuración > Extensiones > fuentes y agregue la ruta de acceso a donde existe ese paquete. Cuando haya terminado la extensión se instala, puede hacer clic en el ```install``` llamará botón y la API.  