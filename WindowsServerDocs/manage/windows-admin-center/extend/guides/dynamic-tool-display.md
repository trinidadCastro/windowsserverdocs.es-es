---
title: Controlar la visibilidad de la herramienta en una solución
description: Controlar la visibilidad de la herramienta en una solución SDK del centro de administración de Windows (proyecto Honolulu)
ms.topic: article
author: nwashburn-ms
ms.author: niwashbu
ms.date: 09/18/2018
ms.localizationpriority: medium
ms.openlocfilehash: df939bb1a87c9ded77431661dcabd7faf607bb6e
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/07/2020
ms.locfileid: "87945028"
---
# <a name="control-your-tools-visibility-in-a-solution"></a>Controlar la visibilidad de la herramienta en una solución #

>Se aplica a: Windows Admin Center, versión preliminar de Windows Admin Center

Puede haber ocasiones en las que desee excluir (u ocultar) la extensión o herramienta de la lista de herramientas disponibles. Por ejemplo, si la herramienta solo tiene como destino Windows Server 2016 (no versiones anteriores), es posible que no quiera que un usuario se conecte a un servidor de Windows Server 2012 R2 para ver la herramienta. (Imagínese la experiencia del usuario: haga clic en ella y espere a que se cargue la herramienta, solo para obtener un mensaje que indica que sus características no están disponibles para su conexión). Puede definir cuándo Mostrar (u ocultar) la característica en el manifest.jsde la herramienta en el archivo.

## <a name="options-for-deciding-when-to-show-a-tool"></a>Opciones para decidir cuándo mostrar una herramienta ##

Hay tres opciones diferentes que puede usar para determinar si la herramienta debe mostrarse y estar disponible para una conexión de clúster o servidor específica.

* localhost
* inventario (una matriz de propiedades)
* script

### <a name="localhost"></a>Host ###

La propiedad localHost del objeto conditions contiene un valor booleano que se puede evaluar como inferencia si el nodo de conexión es localHost (el mismo equipo en el que está instalado el centro de administración de Windows). Al pasar un valor a la propiedad, se indica cuándo (condición) Mostrar la herramienta. Por ejemplo, si solo desea que se muestre la herramienta si el usuario está conectándose al host local, establézcalo de la siguiente manera:

``` json
"conditions": [
{
    "localhost": true
}]
```

Como alternativa, si solo desea que se muestre la herramienta cuando el nodo de conexión *no sea* localhost:

``` json
"conditions": [
{
    "localhost": false
}]
```

Este es el aspecto de las opciones de configuración para mostrar solo una herramienta cuando el nodo que se conecta no es localhost:

``` json
"entryPoints": [
{
    "entryPointType": "tool",
    "name": "main",
    "urlName": "processes",
    "displayName": "resources:strings:displayName",
    "description": "resources:strings:description",
    "icon": "sme-icon:icon-win-serverProcesses",
    "path": "",
    "requirements": [
    {
        "solutionIds": [
        "msft.sme.server-manager!windowsClients"
        ],
        "connectionTypes": [
        "msft.sme.connection-type.windows-client"
        ],
        "conditions": [
        {
            "localhost": true
        }
        ]
    }
    ]
}
```

### <a name="inventory-properties"></a>Propiedades de inventario ###

El SDK incluye un conjunto seleccionada de propiedades de inventario que puede usar para generar condiciones para determinar cuándo debe estar disponible la herramienta. Hay nueve propiedades diferentes en la matriz ' Inventory ':

| Nombre de propiedad | Se esperaba un tipo de valor |
| ------------- | ------------------- |
| computerManufacturer | string |
| operatingSystemSKU | number |
| operatingSystemVersion | version_string (por ejemplo: "10,1. *") |
| productType | number |
| clusterFqdn | string |
| isHyperVRoleInstalled | boolean |
| isHyperVPowershellInstalled | boolean |
| isManagementToolsAvailable | boolean |
| isWmfInstalled | boolean |

Cada objeto de la matriz de inventario debe ajustarse a la siguiente estructura JSON:

``` json
"<property name>": {
    "type": "<expected type>",
    "operator": "<defined operator to use>",
    "value": "<expected value to evaluate using the operator>"
}
```

#### <a name="operator-values"></a>Valores de operador ####

| Operador | Descripción |
| -------- | ----------- |
| gt | mayor que |
| ge | Mayor o igual que |
| lt | menor que |
| le | Menor o igual que |
| eq | igual a |
| ne | not equal to |
| is | comprobar si un valor es true |
| not | comprobar si un valor es false |
| contains | el elemento existe en una cadena |
| notContains | el elemento no existe en una cadena |

#### <a name="data-types"></a>Tipos de datos ####

Opciones disponibles para la propiedad ' tipo ':

| Tipo | Descripción |
| ---- | ----------- |
| version | un número de versión (por ejemplo: 10,1. *) |
| number | un valor numérico |
| string | un valor de cadena |
| boolean | true o false |

#### <a name="value-types"></a>Tipos de valor ####

La propiedad ' value ' acepta estos tipos:

* cadena
* número
* boolean

Un conjunto de condiciones de inventario con el formato correcto tiene el siguiente aspecto:

``` json
"entryPoints": [
{
    "entryPointType": "tool",
    "name": "main",
    "urlName": "processes",
    "displayName": "resources:strings:displayName",
    "description": "resources:strings:description",
    "icon": "sme-icon:icon-win-serverProcesses",
    "path": "",
    "requirements": [
    {
        "solutionIds": [
        "msft.sme.server-manager!servers"
        ],
        "connectionTypes": [
        "msft.sme.connection-type.server"
        ],
        "conditions": [
        {
            "inventory": {
            "operatingSystemVersion": {
                "type": "version",
                "operator": "gt",
                "value": "6.3"
            },
            "operatingSystemSKU": {
                "type": "number",
                "operator": "eq",
                "value": "8"
            }
            }
        }
        ]
    }
    ]
}
```

### <a name="script"></a>Script ###

Por último, puede ejecutar un script de PowerShell personalizado para identificar la disponibilidad y el estado del nodo. Todos los scripts deben devolver un objeto con la siguiente estructura:

``` ps
@{
    State = 'Available' | 'NotSupported' | 'NotConfigured';
    Message = '<Message to explain the reason of state such as not supported and not configured.>';
    Properties =
        @{ Name = 'Prop1'; Value = 'prop1 data'; Type = 'string' },
        @{Name='Prop2'; Value = 12345678; Type='number'; };
}
```
La propiedad State es el valor importante que controlará la decisión de mostrar u ocultar la extensión en la lista de herramientas.  Los valores permitidos son:

| Valor | Descripción |
| ---- | ----------- |
| Disponible | La extensión se debe mostrar en la lista de herramientas. |
| NotSupported | La extensión no se debe mostrar en la lista de herramientas. |
| NoConfigurado | Se trata de un valor de marcador de posición para el trabajo futuro que solicitará al usuario una configuración adicional antes de que la herramienta esté disponible.  Actualmente, este valor hará que se muestre la herramienta y que sea funcional equivalente a "disponible". |

Por ejemplo, si queremos que una herramienta se cargue solo si el servidor remoto tiene instalado BitLocker, el script tiene el siguiente aspecto:

``` ps
$response = @{
    State = 'NotSupported';
    Message = 'Not executed';
    Properties = @{ Name = 'Prop1'; Value = 'prop1 data'; Type = 'string' },
        @{Name='Prop2'; Value = 12345678; Type='number'; };
}

if (Get-Module -ListAvailable -Name servermanager) {
    Import-module servermanager;
    $isInstalled = (Get-WindowsFeature -name bitlocker).Installed;
    $isGood = $isInstalled;
}

if($isGood) {
    $response.State = 'Available';
    $response.Message = 'Everything should work.';
}

$response
```

Una configuración de punto de entrada mediante la opción de script tiene el siguiente aspecto:

``` json
"entryPoints": [
{
    "entryPointType": "tool",
    "name": "main",
    "urlName": "processes",
    "displayName": "resources:strings:displayName",
    "description": "resources:strings:description",
    "icon": "sme-icon:icon-win-serverProcesses",
    "path": "",
    "requirements": [
    {
        "solutionIds": [
        "msft.sme.server-manager!windowsClients"
        ],
        "connectionTypes": [
        "msft.sme.connection-type.windows-client"
        ],
        "conditions": [
        {
            "localhost": true,
            "inventory": {
            "operatingSystemVersion": {
                "type": "version",
                "operator": "eq",
                "value": "10.0.*"
            },
            "operatingSystemSKU": {
                "type": "number",
                "operator": "eq",
                "value": "4"
            }
            },
            "script": "$response = @{ State = 'NotSupported'; Message = 'Not executed'; Properties = @{ Name = 'Prop1'; Value = 'prop1 data'; Type = 'string' }, @{Name='Prop2'; Value = 12345678; Type='number'; }; }; if (Get-Module -ListAvailable -Name servermanager) { Import-module servermanager; $isInstalled = (Get-WindowsFeature -name bitlocker).Installed; $isGood = $isInstalled; }; if($isGood) { $response.State = 'Available'; $response.Message = 'Everything should work.'; }; $response"
        }
        ]
    }
    ]
}
```

## <a name="supporting-multiple-requirement-sets"></a>Compatibilidad con varios conjuntos de requisitos ##

Puede usar más de un conjunto de requisitos para determinar cuándo se debe mostrar la herramienta definiendo varios bloques "requirements".

Por ejemplo, para mostrar la herramienta si "escenario A" o "escenario B" es true, defina dos bloques de requisitos: Si es true (es decir, se cumplen todas las condiciones dentro de un bloque de requisitos), se muestra la herramienta.

``` json
"entryPoints": [
{
    "requirements": [
    {
        "solutionIds": [
            …"scenario A"…
        ],
        "connectionTypes": [
            …"scenario A"…
        ],
        "conditions": [
            …"scenario A"…
        ]
    },
    {
        "solutionIds": [
            …"scenario B"…
        ],
        "connectionTypes": [
            …"scenario B"…
        ],
        "conditions": [
            …"scenario B"…
        ]
    }
    ]
}

```

## <a name="supporting-condition-ranges"></a>Intervalos de condiciones de compatibilidad ##

También puede definir un intervalo de condiciones definiendo varios bloques "conditions" con la misma propiedad, pero con distintos operadores.

Cuando se define la misma propiedad con distintos operadores, se muestra la herramienta siempre que el valor se encuentra entre las dos condiciones.

Por ejemplo, esta herramienta se muestra siempre que el sistema operativo sea una versión entre 6.3.0 y 10.0.0:

``` json
"entryPoints": [
{
    "entryPointType": "tool",
    "name": "main",
    "urlName": "processes",
    "displayName": "resources:strings:displayName",
    "description": "resources:strings:description",
    "icon": "sme-icon:icon-win-serverProcesses",
    "path": "",
    "requirements": [
    {
        "solutionIds": [
             "msft.sme.server-manager!servers"
        ],
        "connectionTypes": [
             "msft.sme.connection-type.server"
        ],
        "conditions": [
        {
            "inventory": {
                "operatingSystemVersion": {
                    "type": "version",
                    "operator": "gt",
                    "value": "6.3.0"
                },
            }
        },
        {
            "inventory": {
                "operatingSystemVersion": {
                    "type": "version",
                    "operator": "lt",
                    "value": "10.0.0"
                }
            }
        }
        ]
    }
    ]
}

```
