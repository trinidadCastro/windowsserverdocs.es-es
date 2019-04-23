---
title: Controlar la visibilidad de la herramienta en una solución
description: Controlar la visibilidad de la herramienta en una solución Windows Admin Center SDK (proyecto Honolulu)
ms.technology: manage
ms.topic: article
author: nwashburn-ms
ms.author: niwashbu
ms.date: 09/18/2018
ms.localizationpriority: medium
ms.prod: windows-server-threshold
ms.openlocfilehash: f3f34b4c86854bfc55cf4b1b57a0fd3c2baf2ffc
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59839256"
---
# <a name="control-your-tools-visibility-in-a-solution"></a>Controlar la visibilidad de la herramienta en una solución #

>Se aplica a: Windows Admin Center, vista previa de Windows Admin Center

Puede haber ocasiones cuando desee excluir (u ocultar) su extensión o la herramienta de la lista de herramientas disponibles. Por ejemplo, si la herramienta está destinada solo Windows Server 2016 (versiones anteriores no), es posible que no desea un usuario que se conecta a un servidor de Windows Server 2012 R2 para ver la herramienta en absoluto. (Imagine: la experiencia del usuario haga clic en él, espere a que la herramienta para cargar, solo para recibir un mensaje que sus características no están disponibles para su conexión.) Puede definir cuándo se debe mostrar la característica en el archivo manifest.json de la herramienta (u ocultar).

## <a name="options-for-deciding-when-to-show-a-tool"></a>Opciones para decidir cuándo se debe mostrar una herramienta ##

Hay tres opciones distintas que puede usar para determinar si la herramienta debe estar muestran y están disponibles para una conexión de clúster o servidor específico.

* localhost
* inventario (una matriz de propiedades)
* secuencia de comandos

### <a name="localhost"></a>LocalHost ###

La propiedad de host local del objeto condiciones contiene un valor booleano que se puede evaluar para deducir si el nodo de conexión es el host local (el mismo equipo que está instalado Windows Admin Center en) o no. Al pasar un valor a la propiedad, indica cuándo (condición) para mostrar la herramienta. Por ejemplo, si solo desea que la herramienta para mostrar si el usuario en realidad se está conectando al host local, configurarla similar al siguiente:

``` json
"conditions": [
{
    "localhost": true
}]
```

Como alternativa, si sólo desea su herramienta para mostrar cuando el nodo conexión *no* localhost:

``` json
"conditions": [
{
    "localhost": false
}]
```

Aquí está la configuración de aspecto para mostrar solo una herramienta cuando el nodo de conexión no es localhost:

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

El SDK incluye un conjunto mantenido previamente de las propiedades del inventario que puede usar para generar las condiciones para determinar cuándo la herramienta debe estar disponible o no. Hay nueve propiedades diferentes de la matriz 'inventariar':

| Nombre de la propiedad | Tipo de valor esperado |
| ------------- | ------------------- |
| computerManufacturer | string |
| operatingSystemSKU | número |
| operatingSystemVersion | version_string (p. ej.: "10.1.*") |
| productType | número |
| clusterFqdn | string |
| isHyperVRoleInstalled | boolean |
| isHyperVPowershellInstalled | boolean |
| isManagementToolsAvailable | boolean |
| isWmfInstalled | boolean |

La siguiente estructura json deben cumplir todos los objetos de la matriz de inventario:

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
| gt | Mayor que |
| ge | Mayor o igual que |
| lt | Menor que |
| le | Menor o igual a |
| eq | Es igual a |
| ne | No es igual a |
| estará | Comprobando si el valor es true |
| not | Comprobando si el valor es false |
| Contiene | el elemento existe en una cadena |
| notContains | elemento no existe en una cadena |

#### <a name="data-types"></a>Tipos de datos ####

Opciones disponibles para la propiedad 'type':

| Tipo | Descripción |
| ---- | ----------- |
| version | un número de versión (p. ej.: 10.1.*) |
| número | un valor numérico |
| string | un valor de cadena |
| boolean | True o false |

#### <a name="value-types"></a>Tipos de valor ####

La propiedad 'value' acepta estos tipos:

* string
* número
* boolean

Un conjunto de condiciones de inventario bien formado tiene este aspecto:

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

Por último, puede ejecutar un script de PowerShell para identificar la disponibilidad y el estado del nodo. Todas las secuencias de comandos deben devolver un objeto con la estructura siguiente:

``` ps
@{
    State = 'Available' | 'NotSupported' | 'NotConfigured';
    Message = '<Message to explain the reason of state such as not supported and not configured.>';
    Properties =
        @{ Name = 'Prop1'; Value = 'prop1 data'; Type = 'string' },
        @{Name='Prop2'; Value = 12345678; Type='number'; };
}
```
La propiedad State es el valor importante que controle la decisión para mostrar u ocultar la extensión en la lista de herramientas.  Los valores permitidos son:
| Valor | Descripción |
| ---- | ----------- |
| Disponible | La extensión se debe mostrar en la lista de herramientas. |
| NotSupported | La extensión no debe mostrarse en la lista de herramientas. |
| NotConfigured | Se trata de un valor de marcador de posición para el trabajo futuro que solicitará al usuario para la configuración adicional antes de la herramienta está disponible.  Actualmente, este valor dará como resultado de la herramienta que se muestran y es el equivalente funcional a 'Disponible'. |

Por ejemplo, si queremos una herramienta para cargar solo si el servidor remoto tiene instalado de BitLocker, la secuencia de comandos este aspecto:

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

Una configuración de punto de entrada mediante la opción de script tiene este aspecto:

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

## <a name="supporting-multiple-requirement-sets"></a>Compatibilidad con varios conjuntos de requisito ##

Puede usar más de un conjunto de requisitos para determinar cuándo se debe mostrar su herramienta definiendo varios bloques "requisitos".

Por ejemplo, para mostrar la herramienta si "escenario A" o "escenario B" es true, defina dos bloques de los requisitos; Si se cumple cualquiera (es decir, se cumplen todas las condiciones dentro de un bloque de requisitos), se muestra la herramienta.

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

## <a name="supporting-condition-ranges"></a>Compatibilidad con los intervalos de condición ##

También puede definir un intervalo de condiciones definiendo varios bloques "condiciones" con la misma propiedad, pero con diversos operadores.

Cuando se define la misma propiedad con distintos operadores, se muestra la herramienta siempre y cuando el valor está comprendido entre las dos condiciones.

Por ejemplo, se muestra esta herramienta siempre y cuando el sistema operativo es una versión entre 6.3.0 y 10.0.0:

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
