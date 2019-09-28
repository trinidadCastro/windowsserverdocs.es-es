---
title: Crear un proveedor de conexión para una extensión de solución
description: 'Desarrollo de una extensión de solución SDK del centro de administración de Windows (proyecto Honolulu): creación de un proveedor de conexiones'
ms.technology: manage
ms.topic: article
author: nwashburn-ms
ms.author: niwashbu
ms.date: 06/06/2019
ms.localizationpriority: medium
ms.prod: windows-server
ms.openlocfilehash: 9c04db3196d1e806e50af9164b3c8bcdfb19b079
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71406889"
---
# <a name="create-a-connection-provider-for-a-solution-extension"></a>Crear un proveedor de conexión para una extensión de solución

>Se aplica a: Windows Admin Center, versión preliminar de Windows Admin Center

Los proveedores de conexión desempeñan un papel importante en el modo en que el centro de administración de Windows define y se comunica con objetos o destinos conectables. Principalmente, un proveedor de conexiones realiza acciones mientras se realiza una conexión, como asegurarse de que el destino está en línea y disponible, y también garantiza que el usuario que se conecta tiene permiso para obtener acceso al destino.

De forma predeterminada, el centro de administración de Windows se distribuye con los siguientes proveedores de conexión:

* Servidor
* Cliente de Windows
* Clúster de conmutación por error
* Clúster de HCI

Para crear su propio proveedor de conexión personalizado, siga estos pasos:

* Agregar detalles del proveedor de conexión a```manifest.json```
* Definir el proveedor de estado de conexión
* Implementar el proveedor de conexiones en el nivel de aplicación

## <a name="add-connection-provider-details-to-manifestjson"></a>Agregar detalles del proveedor de conexión a manifest. JSON

Ahora veremos lo que necesita saber para definir un proveedor de conexión en el archivo del ```manifest.json``` proyecto.

### <a name="create-entry-in-manifestjson"></a>Crear entrada en manifest. JSON

El ```manifest.json``` archivo se encuentra en la carpeta \src y contiene, entre otras cosas, definiciones de puntos de entrada en el proyecto. Los tipos de puntos de entrada incluyen herramientas, soluciones y proveedores de conexión. Vamos a definir un proveedor de conexión.

A continuación se muestra un ejemplo de una entrada de proveedor de conexión en manifest. JSON:

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

Un punto de entrada de tipo "connnectionProvider" indica al shell del centro de administración de Windows que el elemento que se está configurando es un proveedor que usará una solución para validar un estado de conexión. Los puntos de entrada del proveedor de conexión contienen varias propiedades importantes, que se definen a continuación:

| Property | Descripción |
| -------- | ----------- |
| entryPointType | Esta es una propiedad obligatoria. Hay tres valores válidos: "Tool", "Solution" y "connectionProvider". | 
| name | Identifica el proveedor de conexión dentro del ámbito de una solución. Este valor debe ser único dentro de una instancia completa del centro de administración de Windows (no solo una solución). |
| ruta de acceso | Representa la ruta de acceso de la dirección URL para la interfaz de usuario "agregar conexión", si se va a configurar mediante la solución. Este valor se debe asignar a una ruta configurada en el archivo app-Routing. Module. TS. Cuando el punto de entrada de la solución está configurado para usar las conexiones rootNavigationBehavior, esta ruta cargará el módulo que usa el shell para mostrar la interfaz de usuario de la conexión. Más información disponible en la sección sobre rootNavigationBehavior. |
| displayName | El valor especificado aquí se muestra en el lado derecho del shell, debajo de la barra negra del centro de administración de Windows cuando un usuario carga la página de conexiones de una solución. |
| icono | Representa el icono usado en el menú desplegable soluciones para representar la solución. |
| description | Escriba una breve descripción del punto de entrada. |
| connectionType | Representa el tipo de conexión que cargará el proveedor. El valor especificado aquí también se utilizará en el punto de entrada de la solución para especificar que la solución puede cargar esas conexiones. El valor especificado aquí también se utilizará en los puntos de entrada de la herramienta para indicar que la herramienta es compatible con este tipo. Este valor especificado aquí también se utilizará en el objeto de conexión que se envía a la llamada RPC en la "agregar ventana", en el paso de implementación de la capa de aplicación. |
| Tipo | Se usa en la tabla de conexiones para representar una conexión que utiliza el proveedor de conexión. Se espera que sea el nombre en plural del tipo. |
| connectionTypeUrlName | Se usa en la creación de la dirección URL para representar la solución cargada, después de que el centro de administración de Windows se haya conectado a una instancia de. Esta entrada se utiliza después de las conexiones y antes del destino. En este ejemplo, "connectionexample" es donde aparece este valor en la dirección URL:`http://localhost:6516/solutionexample/connections/connectionexample/con-fake1.corp.contoso.com` |
| connectionTypeDefaultSolution | Representa el componente predeterminado que debe cargar el proveedor de conexión. Este valor es una combinación de: <br>[a] el nombre del paquete de extensión definido en la parte superior del manifiesto; <br>[b] signo de exclamación (!); <br>[c] nombre del punto de entrada de la solución.    <br>En el caso de un proyecto con el nombre "msft. SME. solutionExample-Extension" y un punto de entrada de la solución con el nombre "example", este valor sería "msft. SME.-Extension! example". |
| connectionTypeDefaultTool | Representa la herramienta predeterminada que se debe cargar en una conexión correcta. Este valor de propiedad se compone de dos partes, similar a connectionTypeDefaultSolution. Este valor es una combinación de: <br>[a] el nombre del paquete de extensión definido en la parte superior del manifiesto; <br>[b] signo de exclamación (!); <br>[c] el nombre del punto de entrada de la herramienta que se debe cargar inicialmente. <br>En el caso de un proyecto con el nombre "msft. SME. solutionExample-Extension" y un punto de entrada de la solución con el nombre "example", este valor sería "msft. SME. solutionExample-Extension! example". |
| connectionStatusProvider | Vea la sección "definir el proveedor de estado de conexión". |

## <a name="define-connection-status-provider"></a>Definir el proveedor de estado de conexión

El proveedor de estado de conexión es el mecanismo por el que se valida que un destino esté en línea y disponible, asegurándose también de que el usuario que se conecta tiene permiso para obtener acceso al destino. Actualmente hay dos tipos de proveedores de estado de conexión:  PowerShell y RelativeGatewayUrl.

*   <strong>Proveedor de estado de conexión de PowerShell</strong> : determina si un destino está en línea y accesible con un script de PowerShell. El resultado se debe devolver en un objeto con una sola propiedad "status", que se define a continuación.
*   <strong>Proveedor de estado de conexión de RelativeGatewayUrl</strong> : determina si un destino está en línea y accesible con una llamada REST. El resultado se debe devolver en un objeto con una sola propiedad "status", que se define a continuación.

### <a name="define-status"></a>Definir el estado

Los proveedores de estado de conexión son necesarios para devolver un objeto con ```status``` una sola propiedad que se ajusta al formato siguiente:

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

* <strong>Etiqueta</strong> : etiqueta que describe el tipo de valor devuelto de estado. Tenga en cuenta que los valores de la etiqueta se pueden asignar en tiempo de ejecución. Vea la entrada siguiente para asignar valores en tiempo de ejecución.

* <strong>Tipo</strong> : el tipo de valor devuelto de estado. El tipo tiene los siguientes valores de enumeración. Para cualquier valor 2 o superior, la plataforma no navegará hasta el objeto conectado y se mostrará un error en la interfaz de usuario.

   Distintos

  | Valor | Descripción |
  | ----- | ----------- |
  | 0 | Online |
  | 1 | Advertencia |
  | 2 | Sin autorización |
  | 3 | Error |
  | 4 | Crítico |
  | 5 | Unknown |

* <strong>Detalles</strong> : detalles adicionales que describen el tipo de valor devuelto de estado.

### <a name="powershell-connection-status-provider-script"></a>Script del proveedor de estado de conexión de PowerShell

El script de PowerShell del proveedor de estado de conexión determina si un destino está en línea y accesible con un script de PowerShell. El resultado se debe devolver en un objeto con una sola propiedad "status". A continuación se muestra un script de ejemplo.

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

### <a name="define-relativegatewayurl-connection-status-provider-method"></a>Definir método de proveedor de estado de conexión de RelativeGatewayUrl

El método de proveedor ```RelativeGatewayUrl``` de estado de conexión llama a una API de REST para determinar si un destino está en línea y accesible. El resultado se debe devolver en un objeto con una sola propiedad "status". A continuación se muestra un ejemplo de entrada de proveedor de conexión en manifest. JSON de un RelativeGatewayUrl.

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

* "relativeGatewayUrl" especifica dónde obtener el estado de conexión de una dirección URL de puerta de enlace. Este URI es relativo a/API. Si $connectionName se encuentra en la dirección URL, se reemplazará por el nombre de la conexión.
* Todas las propiedades de relativeGatewayUrl deben ejecutarse en la puerta de enlace del host, lo que se puede lograr mediante la creación de una extensión de puerta de enlace.

### <a name="map-values-in-runtime"></a>Asignar valores en tiempo de ejecución

Se puede dar formato a los valores de etiqueta y detalles del objeto de devolución de estado en tiempo de optimización incluyendo las claves y los valores en la propiedad "defaultValueMap" del proveedor.

Por ejemplo, si agrega el valor siguiente, siempre que "defaultConnection_test" se muestre como un valor de etiqueta o de detalles, el centro de administración de Windows reemplazará automáticamente la clave con el valor de cadena de recurso configurado.

``` json
    "defaultConnection_test": "resources:strings:addServer_status_defaultConnection_label"
```

## <a name="implement-connection-provider-in-application-layer"></a>Implementar el proveedor de conexiones en el nivel de aplicación

Ahora vamos a implementar el proveedor de conexión en el nivel de aplicación mediante la creación de una clase TypeScript que implementa OnInit. La clase tiene las siguientes funciones:

| Función | Descripción |
| -------- | ----------- |
| constructor (Private appContextService: AppContextService, ruta privada: ActivatedRoute) |  |
| Public ngOnInit () |  |
| Public onSubmit () | Contiene la lógica para actualizar el shell cuando se realiza un intento de agregar conexión |
| Public OnCancel () | Contiene la lógica para actualizar el shell cuando se cancela un intento de agregar conexión |

### <a name="define-onsubmit"></a>Definir onSubmit

```onSubmit```emite una llamada RPC al contexto de la aplicación para notificar a la shell de una "agregar conexión". La llamada básica utiliza "updateData" similar a la siguiente:

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

### <a name="define-oncancel"></a>Definición de OnCancel

```onCancel```cancela un intento de "agregar conexión" pasando una matriz de conexiones vacía:

``` ts
this.appContextService.rpc.updateData(EnvironmentModule.nameOfShell, '##', <RpcUpdateData>{ results: { connections: [] } });
```

## <a name="connection-provider-example"></a>Ejemplo de proveedor de conexión

A continuación se muestra la clase de TypeScript completa para implementar un proveedor de conexiones. Tenga en cuenta que la cadena "connectionType" coincide con el "connectionType" tal y como se define en el proveedor de conexión en manifest. JSON.

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
