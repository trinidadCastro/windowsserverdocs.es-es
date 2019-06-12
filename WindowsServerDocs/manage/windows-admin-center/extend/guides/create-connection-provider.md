---
title: Crear un proveedor de conexión para una extensión de la solución
description: 'Desarrollar una extensión de la solución Windows Admin Center SDK (proyecto Honolulu): creación de un proveedor de conexión'
ms.technology: manage
ms.topic: article
author: nwashburn-ms
ms.author: niwashbu
ms.date: 06/06/2019
ms.localizationpriority: medium
ms.prod: windows-server-threshold
ms.openlocfilehash: b79e832ee45990d18baf4c211ab68b907134ceb7
ms.sourcegitcommit: 6ef4986391607bb28593852d06cc6645e548a4b3
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 06/07/2019
ms.locfileid: "66811834"
---
# <a name="create-a-connection-provider-for-a-solution-extension"></a>Crear un proveedor de conexión para una extensión de la solución

>Se aplica a: Windows Admin Center, vista previa de Windows Admin Center

Proveedores de conexión desempeñan un papel importante en cómo Windows Admin Center define y se comunica con los objetos conectables o destinos. Principalmente, un proveedor de conexión realiza acciones mientras se realiza una conexión, como asegurarse de que el destino está en línea y disponibles y también lo que garantiza que el usuario que se conecta tiene permiso para tener acceso al destino.

De forma predeterminada, Windows Admin Center incluye los siguientes proveedores de conexión:

* Servidor
* Cliente de Windows
* Clúster de conmutación por error
* Clúster de HCL

Para crear su propio proveedor de conexión personalizado, siga estos pasos:

* Agregar detalles del proveedor de conexión para ```manifest.json```
* Defina el proveedor de estado de conexión
* Implementar el proveedor de conexión de capa de aplicación

## <a name="add-connection-provider-details-to-manifestjson"></a>Agregar detalles del proveedor de conexión a manifest.json

Ahora le guiaremos a través de lo que necesita saber para definir un proveedor de conexión en el proyecto ```manifest.json``` archivo.

### <a name="create-entry-in-manifestjson"></a>Crear entrada en manifest.json

El ```manifest.json``` archivo se encuentra en la carpeta \src y contiene, entre otras cosas, las definiciones de puntos de entrada en el proyecto. Tipos de puntos de entrada incluyen herramientas, soluciones y proveedores de conexión. Se va a definir un proveedor de conexión.

Un ejemplo de una entrada del proveedor de conexión en manifest.json está por debajo:

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

Un punto de entrada de tipo "connnectionProvider" indica al shell de Windows Admin Center que el elemento que se está configurando es un proveedor que se usará para validar un estado de conexión por una solución. Puntos de entrada del proveedor de conexión contiene un número de propiedades importantes, que se definen a continuación:

| Property | Descripción |
| -------- | ----------- |
| entryPointType | Se trata de una propiedad necesaria. Hay tres valores válidos: "herramienta", "solución" y "connectionProvider". | 
| name | Identifica el proveedor de conexión dentro del ámbito de una solución. Este valor debe ser único dentro de una instancia completa de Windows Admin Center (no sólo una solución). |
| ruta de acceso | Representa la ruta de acceso de dirección URL para el "Agregar conexión" interfaz de usuario, si se configurará la solución. Este valor debe asignar a una ruta que se configura en el archivo de aplicación routing.module.ts. Cuando el punto de entrada de la solución está configurado para usar el rootNavigationBehavior conexiones, esta ruta cargará el módulo que se usa el shell para mostrar la interfaz de usuario de conexión de agregar. Más información disponible en la sección sobre rootNavigationBehavior. |
| displayName | El valor introducido aquí se muestra en el lado derecho del shell, debajo de la barra negra de Windows Admin Center cuando un usuario carga la página de conexiones de la solución. |
| icono | Representa el icono utilizado en el menú desplegable de soluciones para representar la solución. |
| description | Escriba una breve descripción del punto de entrada. |
| connectionType | Representa el tipo de conexión que se cargará el proveedor. El valor especificado aquí también se usará en el punto de entrada de la solución para especificar que la solución puede cargar esas conexiones. El valor especificado aquí también se utilizará en la herramienta o los puntos de entrada para indicar que la herramienta es compatible con este tipo. Este valor introducido aquí se usará también en el objeto de conexión que se envía a la RPC llamar en la "ventana de agregar", en el paso de implementación de capa de aplicación. |
| connectionTypeName | Se utiliza en la tabla de conexiones para representar una conexión que utiliza el proveedor de la conexión. Esto se espera que el nombre plural del tipo. |
| connectionTypeUrlName | Se utiliza en la creación de la dirección URL para representar la solución cargada, después de que Windows Admin Center se ha conectado a una instancia. Esta entrada se utiliza después de las conexiones y antes del destino. En este ejemplo, "connectionexample" es donde este valor aparece en la dirección URL: `http://localhost:6516/solutionexample/connections/connectionexample/con-fake1.corp.contoso.com` |
| connectionTypeDefaultSolution | Representa el componente predeterminado que se debe cargar el proveedor de conexión. Este valor es una combinación de: <br>[a] el nombre del paquete de extensión definido en la parte superior del manifiesto; <br>signo de exclamación (!); [b] <br>[c], el nombre de punto de entrada de solución.    <br>Para un proyecto con el nombre "msft.sme.mySample-extension" y un punto de entrada de la solución con el nombre "example", este valor sería "msft.sme.solutionExample extensión! ejemplo". |
| connectionTypeDefaultTool | Representa el valor predeterminado de herramienta que se debe cargar en una conexión correcta. Este valor de propiedad se compone de dos partes, similares a la connectionTypeDefaultSolution. Este valor es una combinación de: <br>[a] el nombre del paquete de extensión definido en la parte superior del manifiesto; <br>signo de exclamación (!); [b] <br>[c], el nombre del punto de entrada de herramienta para la herramienta que se debe cargar inicialmente. <br>Para un proyecto con el nombre "msft.sme.solutionExample-extension" y un punto de entrada de la solución con el nombre "example", este valor sería "msft.sme.solutionExample extensión! ejemplo". |
| connectionStatusProvider | Consulte la sección "Definir proveedor de estado de la conexión" |

## <a name="define-connection-status-provider"></a>Defina el proveedor de estado de conexión

Proveedor de estado de conexión es el mecanismo por el que se valida un destino para estar en línea y disponibles, también lo que garantiza que el usuario que se conecta tiene permiso para tener acceso al destino. Actualmente hay dos tipos de proveedores de estado de conexión:  PowerShell y RelativeGatewayUrl.

*   <strong>Proveedor de estado de conexión de PowerShell</strong> -determina si un destino está en línea y accesible con un script de PowerShell. El resultado se debe devolver en un objeto con una sola propiedad "estado", que define a continuación.
*   <strong>El proveedor de estado de conexión RelativeGatewayUrl</strong> -determina si un destino está en línea y accesible con una llamada de rest. El resultado se debe devolver en un objeto con una sola propiedad "estado", que define a continuación.

### <a name="define-status"></a>Definir estado

Proveedores de estado de conexión son necesarios para devolver un objeto con una sola propiedad ```status``` que se ajusta al formato siguiente:

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

* <strong>Etiqueta</strong> : una etiqueta que describe el tipo de valor devuelto del estado. Tenga en cuenta que se pueden asignar valores de etiqueta en tiempo de ejecución. Consulte la entrada siguiente para asignar valores en tiempo de ejecución.

* <strong>Tipo</strong> -tipo de valor devuelto del estado. Tipo tiene los siguientes valores de enumeración. Para cualquier valor de 2 o posterior, la plataforma no se le remitirá a objeto conectado y se mostrará un error en la interfaz de usuario.

   Tipos:

  | Valor | Descripción |
  | ----- | ----------- |
  | 0 | Online |
  | 1 | Advertencia |
  | 2 | Sin autorización |
  | 3 | Error |
  | 4 | Fatal |
  | 5 | Unknown |

* <strong>Detalles</strong> : detalles adicionales que describe el tipo de valor devuelto del estado.

### <a name="powershell-connection-status-provider-script"></a>Secuencia de comandos de proveedor de estado de conexión de PowerShell

El script de PowerShell de proveedor de estado de conexión determina si un destino está en línea y accesible con un script de PowerShell. El resultado se debe devolver en un objeto con una sola propiedad "estado". A continuación se muestra un script de ejemplo.

Script de PowerShell de ejemplo:

```PowerShell
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

### <a name="define-relativegatewayurl-connection-status-provider-method"></a>Definir el método de proveedor de estado de conexión RelativeGatewayUrl

El proveedor de estado de conexión ```RelativeGatewayUrl``` método llama a una API para determinar si un destino está en línea y accesibles de rest. El resultado se debe devolver en un objeto con una sola propiedad "estado". Un ejemplo de la entrada del proveedor de conexión en manifest.json de un RelativeGatewayUrl se muestra a continuación.

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

* "relativeGatewayUrl" especifica dónde obtener el estado de conexión desde una dirección URL de puerta de enlace. Este URI es relativa con respecto a/API. Si $connectionName se encuentra en la dirección URL, se reemplazará con el nombre de la conexión.
* Todas las propiedades de relativeGatewayUrl deben ejecutarse con la puerta de enlace de host, que puede realizarse mediante la creación de una extensión de la puerta de enlace

### <a name="map-values-in-runtime"></a>Asignar los valores de tiempo de ejecución

Los valores de etiqueta y los detalles en el estado del objeto de valor devuelto se puede dar formato al optimización el tiempo mediante la inclusión de las claves y valores en la propiedad "defaultValueMap" del proveedor.

Por ejemplo, si agrega el siguiente valor, siempre que ese "defaultConnection_test" aparecían como un valor para la etiqueta o detalles, Windows Admin Center reemplazará automáticamente la clave con el valor de cadena de recursos configurados.

``` json
    "defaultConnection_test": "resources:strings:addServer_status_defaultConnection_label"
```

## <a name="implement-connection-provider-in-application-layer"></a>Implementar el proveedor de conexión de capa de aplicación

Ahora vamos a implementar el proveedor de conexión en el nivel de aplicación, mediante la creación de una clase TypeScript que implementa OnInit. La clase tiene las siguientes funciones:

| Función | Descripción |
| -------- | ----------- |
| constructor(private appContextService: AppContextService, ruta privada: ActivatedRoute) |  |
| public ngOnInit() |  |
| onSubmit() pública | Contiene la lógica para actualizar el shell cuando se realiza un intento de agregar conexión |
| onCancel() pública | Contiene la lógica para actualizar el shell cuando se cancela un intento de agregar conexión |

### <a name="define-onsubmit"></a>Definir onSubmit

```onSubmit``` problemas de una llamada RPC volver a llamar en el contexto de la aplicación para notificar al shell de una "conexión agregar". La llamada básica usa "updateData" similar al siguiente:

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

El resultado es una propiedad de conexión, que es una matriz de objetos que se ajustan a la estructura siguiente:

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

### <a name="define-oncancel"></a>Definir onCancel

```onCancel``` cancela un intento de "Agregar conexión" pasando una matriz vacía de conexiones:

``` ts
this.appContextService.rpc.updateData(EnvironmentModule.nameOfShell, '##', <RpcUpdateData>{ results: { connections: [] } });
```

## <a name="connection-provider-example"></a>Ejemplo de proveedor de conexión

La clase completa de TypeScript para implementar un proveedor de conexión está por debajo. Tenga en cuenta que la cadena "connectionType" coincide con el "tipo de conexión como se define en el proveedor de conexión en manifest.json.

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
