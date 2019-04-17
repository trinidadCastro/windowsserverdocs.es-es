---
title: Desarrollar una extensión de herramienta
description: Desarrollar una extensión de herramienta SDK de Windows Admin Center (proyecto Honolulu)
ms.technology: manage
ms.topic: article
author: nwashburn-ms
ms.author: niwashbu
ms.date: 09/18/2018
ms.localizationpriority: medium
ms.prod: windows-server-threshold
ms.openlocfilehash: 2269a2ac2cabda6f8fdd829994f36e89d581bd23
ms.sourcegitcommit: f1edfc6525e09dd116b106293f9260123a94de0c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/12/2019
ms.locfileid: "9297103"
---
# Instalar la carga de extensión en un nodo administrado

>Se aplica a: Windows Admin Center, Versión preliminar de Windows Admin Center

## Programa de instalación
> [!NOTE]
> Para seguir esta guía, necesitas crear 1.2.1904.02001 o superior. Para comprobar la compilación número abre Windows Admin Center y haz clic en el signo de interrogación en la esquina superior derecha.

Si no lo has hecho ya, cree una [extensión de herramienta](../develop-tool.md) para Windows Admin Center. Después de completar esta marca nota de los valores usados al crear una extensión:
| Valor | Explicación | Ejemplo |
| ----- | ----------- | ------- |
| ```{!Company Name}``` | Nombre de tu compañía (con espacios) | ```Contoso``` |
| ```{!Tool Name}``` | El nombre de la herramienta (con espacios) | ```InstallOnNode``` |

Dentro de la carpeta de extensión de herramienta, cree una ```Node``` carpeta (```{!Tool Name}\Node```). Nada colocados en esta carpeta se copiarán en el nodo administrado al usar esta API. Agregar los archivos necesarios para el caso de uso. 

Crear también un ```{!Tool Name}\Node\installNode.ps1``` script. Este script se debe ejecutar en el nodo administrado una vez que se copian todos los archivos desde el ```{!Tool Name}\Node``` carpeta con el nodo administrado. Agregue cualquier lógica adicional para el caso de uso. Un ejemplo ```{!Tool Name}\Node\installNode.ps1``` archivo:

``` ps1
# Add logic for installing payload on managed node
echo 'Success'
```

> [!NOTE]
> ```{!Tool Name}\Node\installNode.ps1``` tiene un nombre específico que buscará la API. Cambiar el nombre de este archivo se producirá un error.


## Integración con la interfaz de usuario

Actualización ```\src\app\default.component.ts``` a la siguiente:

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
Actualizar los marcadores de posición con los valores que se usaron al crear la extensión:
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

También tendrán que actualizar ```\src\app\default.component.html``` para:
``` html
<button (click)="installOnNode()">Click to install</button>
<sme-loading-wheel *ngIf="loading" size="large"></sme-loading-wheel>
<p *ngIf="response">{{response}}</p>
```
Y, por último ```\src\app\default.module.ts```:
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

## Crear e instalar un paquete de NuGet

El último paso es compilar un paquete de NuGet con los archivos que hemos agregado y luego instalar dicho paquete en Windows Admin Center.

Si no has creado un paquete de extensión antes, sigue a la Guía de [Publicación de extensiones](../publish-extensions.md) . 
> [!IMPORTANT]
> En el archivo .nuspec para esta extensión, es importante que el ```<id>``` valor coincide con el nombre en el proyecto ```manifest.json``` y ```<version>``` coincide con lo que se ha agregado a ```\src\app\default.component.ts```. También agrega una entrada en ```<files>```: 

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

Una vez que se crea este paquete, agrega una ruta de acceso a ese fuente. En Windows Admin Center ve a configuración > extensiones > fuentes y agregar la ruta de acceso a donde existe ese paquete. Cuando haya terminado la extensión se instalen, deberías poder hacer clic en el ```install``` se llamará botón y la API.  