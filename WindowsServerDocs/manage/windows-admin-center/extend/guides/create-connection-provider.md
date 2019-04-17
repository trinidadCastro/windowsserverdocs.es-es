---
title: Crear un proveedor de conexión para una extensión de la solución
description: 'Desarrollar una extensión de la solución SDK de Windows Admin Center (proyecto Honolulu): crear un proveedor de conexión'
ms.technology: manage
ms.topic: article
author: nwashburn-ms
ms.author: niwashbu
ms.date: 09/18/2018
ms.localizationpriority: medium
ms.prod: windows-server-threshold
ms.openlocfilehash: 883fba96fcb71cb1c6e8162c1564d66924c4e24d
ms.sourcegitcommit: be0144eb59daf3269bebea93cb1c467d67e2d2f1
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/20/2018
ms.locfileid: "4081142"
---
# Crear un proveedor de conexión para una extensión de la solución

>Se aplica a: Windows Admin Center, Versión preliminar de Windows Admin Center

Proveedores de conexión desempeñan un papel importante en la forma en que Windows Admin Center define y se comunica con objetos conectables o destinos. Principalmente, un proveedor de conexión realiza acciones mientras se realiza una conexión, como garantizar que el destino es en línea y disponible y también asegurarse de que el usuario que se conecta tiene permiso para acceder al destino.

De manera predeterminada, Windows Admin Center se incluye con los proveedores de conexión siguientes:

* Servidor
* Cliente de Windows
* Clúster de conmutación por error
* Clúster HCI

Para crear tu propio proveedor personalizado de conexión, sigue estos pasos:

* Agregar detalles de proveedor de la conexión a ```manifest.json```
* Definir el proveedor de estado de conexión
* Implementar el proveedor de la conexión en la capa de la aplicación

## Agregar detalles de proveedor de la conexión a manifest.json

Ahora te guiaremos por lo que necesitas saber para definir un proveedor de conexión en el proyecto ```manifest.json``` archivo.

### Crear entrada en manifest.json

El ```manifest.json``` archivo se encuentra en la carpeta \src y contiene, entre otras cosas, las definiciones de puntos de entrada en el proyecto. Tipos de puntos de entrada incluyen herramientas, soluciones y proveedores de conexión. Te enviaremos a definir un proveedor de conexión.

Es un ejemplo de una entrada de proveedor de la conexión en manifest.json a continuación:

``` json
    {
      "entryPointType": "connectionProvider",
      "name": "addServer",
      "path": "/add",
      "displayName": "resources:strings:addServer_displayName",
      "icon": "sme-icon:icon-win-server",
      "description": "resources:strings:description",
      "connectionType": "msft.sme.connection-type.server",
      "connectionTypeName": "resources:strings:addServer_connectionTypeName",
      "connectionTypeUrlName": "server",
      "connectionTypeDefaultSolution": "msft.sme.server-manager!servers",
      "connectionTypeDefaultTool": "msft.sme.server-manager!overview",
      "connectionStatusProvider": {
        "powerShell": {
          "script": "## Get-My-Status ##\nfunction Get-Status()\n{\n# A function like this would be where logic would exist to identify if a node is connectable.\n$status = @{label = $null; type = 0; details = $null; }\n$caption = \"MyConstCaption\"\n$productType = \"MyProductType\"\n# A result object needs to conform to the following object structure to be interpreted properly by the Windows Admin Center shell.\n$result = @{ status = $status; caption = $caption; productType = $productType; version = $version }\n# DO FANCY LOGIC #\n# Once the logic is complete, the following fields need to be populated:\n$status.label = \"Display Thing\"\n$status.type = 0 # This value needs to conform to the LiveConnectionStatusType enum. >= 3 represents a failure.\n$status.details = \"success stuff\"\nreturn $result}\nGet-Status"
        },
        "displayValueMap": {
          "wmfMissing-label": "resources:strings:addServer_status_wmfMissing_label",
          "wmfMissing-details": "resources:strings:addServer_status_wmfMissing_details",
          "unsupported-label": "resources:strings:addServer_status_unsupported_label",
          "unsupported-details": "resources:strings:addServer_status_unsupported_details"
        }
      }
    },
```

Un punto de entrada del tipo "connnectionProvider" indica al shell de Windows Admin Center que el elemento que se están configurando es un proveedor que se usará por una solución para validar un estado de conexión. Puntos de entrada de proveedor de conexión contiene un número de propiedades importantes, que se define a continuación:

| Propiedad | Descripción |
| -------- | ----------- |
| entryPointType | Se trata de una propiedad necesaria. Hay tres valores válidos: "herramienta", "solución" y "connectionProvider". | 
| name | Identifica el proveedor de conexión dentro del ámbito de una solución. Este valor debe ser único dentro de una instancia de Windows Admin Center completa (no solo una solución). |
| path | Representa la ruta de acceso de dirección URL para la "Agregar conexión" la interfaz de usuario, si se configurará la solución. Este valor debe asignar a una ruta que se configura en el archivo routing.module.ts de la aplicación. Cuando el punto de entrada de la solución está configurado para usar la rootNavigationBehavior conexiones, esta ruta cargará el módulo que se usa el shell para mostrar la interfaz de usuario de conexión de agregar. Más información disponible en la sección sobre rootNavigationBehavior. |
| displayName | El valor introducido aquí se muestra en el lado derecho del shell, debajo de la barra de Windows Admin Center negro cuando un usuario carga la página de conexiones de la solución. |
| icon | Representa el icono usado en el menú desplegable soluciones para representar la solución. |
| description | Escribe una descripción breve de punto de entrada. |
| connectionType | Representa el tipo de conexión que se cargará el proveedor. El valor introducido aquí también se usará en el punto de entrada de la solución para especificar que la solución puede cargar las conexiones. El valor introducido aquí también se usará en puntos de entrada de herramienta para indicar que la herramienta es compatible con este tipo. Este valor introducido aquí también se usarán en el objeto de conexión que se envía a la RPC llamar en la "ventana de agregar", en el paso de implementación de capa de aplicación. |
| connectionTypeName | Se usa en la tabla de conexiones para representar una conexión que usa tu proveedor de conexión. Esto se espera que el nombre del tipo plural. |
| connectionTypeUrlName | Se usa en la creación de la dirección URL para representar la solución cargada, después de que Windows Admin Center se ha conectado a una instancia. Se usa esta entrada después de las conexiones y antes del destino. En este ejemplo, "connectionexample" es donde aparece este valor en la dirección URL:http://localhost:6516/solutionexample/connections/connectionexample/con-fake1.corp.contoso.com |
| connectionTypeDefaultSolution | Representa el componente predeterminado que se debe cargar el proveedor de conexión. Este valor es una combinación de: [a] el nombre del paquete de extensión definido en la parte superior del manifiesto; [b] signo de exclamación (!); [c] el nombre de punto de entrada de solución.    Para un proyecto con nombre "msft.sme.mySample-extensión" y un punto de entrada de la solución con el nombre "example", este valor sería "msft.sme.solutionExample extensión! ejemplo". |
| connectionTypeDefaultTool | Representa el valor predeterminado de herramienta que debe cargarse en una conexión correcta. Este valor de propiedad se compone de dos partes, similares a la connectionTypeDefaultSolution. Este valor es una combinación de: [a] el nombre del paquete de extensión definido en la parte superior del manifiesto; [b] signo de exclamación (!); [c] el nombre del punto de entrada de herramienta para la herramienta que debe cargarse inicialmente. Para un proyecto con nombre "msft.sme.solutionExample-extensión" y un punto de entrada de la solución con el nombre "example", este valor sería "msft.sme.solutionExample extensión! ejemplo". |
| connectionStatusProvider | Consulte la sección "Definir el proveedor del estado de conexión" |

## Definir el proveedor de estado de conexión

Proveedor de estado de conexión es el mecanismo por el cual se valida un destino para que esté en línea y disponible, además de garantizar que el usuario que se conecta tiene permiso para acceder al destino. Actualmente hay dos tipos de proveedores de estado de conexión: PowerShell y RelativeGatewayUrl.

*   Proveedor de estado de conexión de PowerShell
    *   Determina si un destino está en línea y accesibles con un script de PowerShell. El resultado se debe devolver un objeto con una sola propiedad "status", que define a continuación.
*   Proveedor de estado de conexión de RelativeGatewayUrl
    *   Determina si un destino está en línea y accesibles con una llamada de rest. El resultado se debe devolver un objeto con una sola propiedad "status", que define a continuación.

### Definir el estado de

Los proveedores de estado de conexión son necesarios para devolver un objeto con una sola propiedad ```status``` que cumple con el formato siguiente:

``` json
{
    status: {
        label: string;
        type: int;
        details: string;
    }
}
```

Propiedades de estado:

* Etiqueta
    * Una etiqueta que describe el tipo de devolución de estado. Ten en cuenta que se pueden asignar los valores de etiqueta en tiempo de ejecución. Consulte la entrada a continuación para asignar valores en tiempo de ejecución.

* Tipo
    * El tipo devuelto de estado. Tipo tiene los siguientes valores de enumeración. Para cualquier valor de 2 o posterior, la plataforma no navegará a del objeto conectado y se mostrará un error en la interfaz de usuario.

Tipos de:

| Valor | Descripción |
| ----- | ----------- |
| 0 | Online |
| 1 | Advertencia |
| 2 | No autorizado |
| 3 | Error |
| 4 | Grave |
| 5 | Unknown |

* Detalles
    * Detalles adicionales que describe el estado del tipo devuelven.

### Script de proveedor de estado de conexión de PowerShell

El script de PowerShell de proveedor de estado de conexión determina si un destino está en línea y accesibles con un script de PowerShell. El resultado se debe devolver un objeto con una sola propiedad "status". A continuación se muestra un script de ejemplo.

Script de PowerShell de ejemplo:

``` ts
## Get-My-Status ##

function Get-Status()
{
    # A function like this would be where logic would exist to identify if a node is connectable.
    $status = @{label = $null; type = 0; details = $null; }
    $caption = "MyConstCaption"
    $productType = "MyProductType"

    # A result object needs to conform to the following object structure to be interperated properly by the Windows Admin Center shell.
    $result = @{ status = $status; caption = $caption; productType = $productType; version = $version }

    # DO FANCY LOGIC #

    # Once the logic is complete, the following fields need to be populated:
    $status.label = "Display Thing"
    $status.type = 0 # This value needs to conform to the LiveConnectionStatusType enum. >= 3 represents a failure.
    $status.details = "success stuff"

    return $result
}

Get-Status
```

### Definir el método de proveedor de estado de conexión de RelativeGatewayUrl

El proveedor de estado de conexión ```RelativeGatewayUrl``` método llama a una API para determinar si un destino está en línea y accesibles de rest. El resultado se debe devolver un objeto con una sola propiedad "status". Un ejemplo de la entrada del proveedor de conexión de manifest.json de un RelativeGatewayUrl se muestra a continuación.

``` json
    {
      "entryPointType": "connectionProvider",
      "name": "addServer",
      "path": "/add/server",
      "displayName": "resources:strings:addServer_displayName",
      "icon": "sme-icon:icon-win-server",
      "description": "resources:strings:description",
      "connectionType": "msft.sme.connection-type.server",
      "connectionTypeName": "resources:strings:addServer_connectionTypeName",
      "connectionTypeUrlName": "server",
      "connectionTypeDefaultSolution": "msft.sme.server-manager!servers",
      "connectionTypeDefaultTool": "msft.sme.server-manager!overview",
      "connectionStatusProvider": {
        "relativeGatewayUrl": "<URL here post /api>",
        "displayValueMap": {
          "wmfMissing-label": "resources:strings:addServer_status_wmfMissing_label",
          "wmfMissing-details": "resources:strings:addServer_status_wmfMissing_details",
          "unsupported-label": "resources:strings:addServer_status_unsupported_label",
          "unsupported-details": "resources:strings:addServer_status_unsupported_details"
        }
      }
    },
```

Notas sobre el uso de RelativeGatewayUrl:

* "relativeGatewayUrl" especifica dónde se debe obtener el estado de conexión desde una dirección URL de la puerta de enlace. Este URI es relativo desde/API. Si $connectionName se encuentra en la dirección URL, se reemplazará por el nombre de la conexión.
* Se deben ejecutar todas las propiedades de relativeGatewayUrl frente a la puerta de enlace de host, que se puede lograr mediante la creación de una extensión de puerta de enlace

### Asignar los valores de tiempo de ejecución

Los valores de etiqueta y detalles en el estado del objeto devuelto puede formatearse en optimizar el tiempo, las claves y valores en la propiedad "defaultValueMap" del proveedor.

Por ejemplo, si agregas el valor a continuación, en cualquier momento que "defaultConnection_test" mostrado como un valor de etiqueta o detalles, Windows Admin Center se configurará automáticamente reemplaza la clave con el valor de cadena de recursos configurado.

``` json
    "defaultConnection_test": "resources:strings:addServer_status_defaultConnection_label"
```

## Implementar el proveedor de la conexión en la capa de la aplicación

Ahora, vamos a implementar el proveedor de conexión en la capa de la aplicación, mediante la creación de una clase TypeScript que implementa OnInit. La clase tiene las siguientes funciones:

| Función | Descripción |
| -------- | ----------- |
| constructor (appContextService privada: AppContextService, ruta privada: ActivatedRoute) |  |
| ngOnInit() pública |  |
| onSubmit() pública | Contiene la lógica para actualizar el shell cuando se realiza un intento de conexión de agregar |
| onCancel() pública | Contiene la lógica para actualizar el shell cuando se cancela un intento de conexión de agregar |

### Definir onSubmit

```onSubmit``` problemas de una RPC la llamada en el contexto de la aplicación para notificar el shell de un "Agregar conexión". La llamada básica utiliza "updateData" como este:

``` ts
this.appContextService.rpc.updateData(
    EnvironmentModule.nameOfShell,
    '##',
    <RpcUpdateData>{
        results: {
            connections: connections,
            credentials: this.useCredentials ? this.creds : null
        }
    }
);
```

El resultado es una propiedad de conexión, que es una matriz de objetos que se ajusten a la estructura siguiente:

``` ts

/**
 * The connection attributes class.
 */
export interface ConnectionAttribute {

    /**
     * The id string of this attribute
     */
    id: string;

    /**
     * The value of the attribute. used for attributes that can have variable values such as Operating System
     */
    value?: string | number;
}

/**
 * The connection class.
 */
export interface Connection {

    /**
     * The id of the connection, this is unique per connection
     */
    id: string;

    /**
     * The type of connection
     */
    type: string;

    /**
     * The name of the connection, this is unique per connection type
     */
    name: string;

    /**
     * The property bag of the connection
     */
    properties?: ConnectionProperties;

    /**
     * The ids of attributes identified for this connection
     */
    attributes?: ConnectionAttribute[];

    /**
     * The tags the user(s) have assigned to this connection
     */
    tags?: string[];
}

/**
 * Defines connection type strings known by core
 * Be careful that these strings match what is defined by the manifest of @msft-sme/server-manager
 */
export const connectionTypeConstants = {
    server: 'msft.sme.connection-type.server',
    cluster: 'msft.sme.connection-type.cluster',
    hyperConvergedCluster: 'msft.sme.connection-type.hyper-converged-cluster',
    windowsClient: 'msft.sme.connection-type.windows-client',
    clusterNodesProperty: 'nodes'
};
```

### Definir onCancel

```onCancel``` cancela un intento de "Agregar conexión" pasando una matriz de conexiones vacía:

``` ts
this.appContextService.rpc.updateData(EnvironmentModule.nameOfShell, '##', <RpcUpdateData>{ results: { connections: [] } });
```

## Ejemplo de proveedor de conexión

La clase TypeScript completa para implementar un proveedor de conexión está por debajo. Ten en cuenta que la cadena "connectionType" coincide con la "connectionType según se define en el proveedor de conexión en manifest.json.

``` ts
import { Component, OnInit } from '@angular/core';
import { ActivatedRoute } from '@angular/router';
import { AppContextService } from '@msft-sme/shell/angular';
import { Connection, ConnectionUtility } from '@msft-sme/shell/core';
import { EnvironmentModule } from '@msft-sme/shell/dist/core/manifest/environment-modules';
import { RpcUpdateData } from '@msft-sme/shell/dist/core/rpc/rpc-base';
import { Strings } from '../../generated/strings';

@Component({
  selector: 'add-example',
  templateUrl: './add-example.component.html',
  styleUrls: ['./add-example.component.css']
})
export class AddExampleComponent implements OnInit {
  public newConnectionName: string;
  public strings = MsftSme.resourcesStrings<Strings>().SolutionExample;
  private connectionType = 'msft.sme.connection-type.example'; // This needs to match the connectionTypes value used in the manifest.json.
  
  constructor(private appContextService: AppContextService, private route: ActivatedRoute) {
    // TODO:
  }

  public ngOnInit() {
    // TODO
  }

  public onSubmit() {
    let connections: Connection[] = [];

    let connection = <Connection> {
      id: ConnectionUtility.createConnectionId(this.connectionType, this.newConnectionName),
      type: this.connectionType,
      name: this.newConnectionName
    };

    connections.push(connection);

    this.appContextService.rpc.updateData(
      EnvironmentModule.nameOfShell,
      '##',
      <RpcUpdateData> {
        results: {
          connections: connections,
          credentials: null
        }
      }
    );
  }

  public onCancel() {
    this.appContextService.rpc.updateData(
      EnvironmentModule.nameOfShell, '##', <RpcUpdateData>{ results: { connections: [] } });
  }
}

```
