---
title: Controlar la visibilidad de la herramienta en una solución
description: Controlar la visibilidad de la herramienta en una solución de SDK de Windows Admin Center (proyecto Honolulu)
ms.technology: manage
ms.topic: article
author: nwashburn-ms
ms.author: niwashbu
ms.date: 09/18/2018
ms.localizationpriority: medium
ms.prod: windows-server-threshold
ms.openlocfilehash: f3f34b4c86854bfc55cf4b1b57a0fd3c2baf2ffc
ms.sourcegitcommit: be0144eb59daf3269bebea93cb1c467d67e2d2f1
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/20/2018
ms.locfileid: "4080972"
---
# Controlar la visibilidad de la herramienta en una solución #

>Se aplica a: Windows Admin Center, Versión preliminar de Windows Admin Center

Es posible que haya veces cuando quieras excluir (u ocultar) la extensión o la herramienta de la lista de herramientas disponibles. Por ejemplo, si la herramienta está destinada a solo Windows Server 2016 (versiones anteriores no), es posible que no quieres que un usuario que se conecta a un servidor de Windows Server 2012 R2 para ver la herramienta en absoluto. (Imagina la experiencia del usuario - haga clic en él, espera a que la herramienta para cargar, solo para obtener un mensaje que sus características no están disponibles para su conexión.) Puedes definir cuándo se debe mostrar (u ocultar) la característica en el archivo de manifest.json de la herramienta.

## Opciones para decidir cuándo se debe mostrar una herramienta ##

Existen tres opciones diferentes que puedes usar para determinar si la herramienta se debe mostrado y disponibles para una conexión de clúster o servidor específico.

* localhost
* inventario (una matriz de propiedades)
* secuencia de comandos

### LocalHost ###

La propiedad localHost del objeto condiciones contiene un valor booleano que se puede evaluar para deducir si el nodo de conexión es localHost (el mismo equipo que está instalado Windows Admin Center en) o no. Al pasar un valor a la propiedad, debes indicar cuándo (la condición) para mostrar la herramienta. Por ejemplo, si quieres que solo la herramienta que se mostrará si el usuario en realidad se conecta al host local, configurarlo como este:

``` json
"conditions": [
{
    "localhost": true
}]
```

Como alternativa, si solo quieres que tu herramienta a mostrar cuando la conexión localhost *no es* de nodo:

``` json
"conditions": [
{
    "localhost": false
}]
```

Esta es la configuración de aspecto para mostrar solo una herramienta cuando el nodo de conexión no está localhost:

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

### Propiedades de inventario ###

El SDK incluye un conjunto ajustado previamente de propiedades de inventario que puedes usar para generar las condiciones para determinar cuando la herramienta debe ser disponible o no. Hay nueve propiedades diferentes en la matriz de 'inventario':

| Nombre de propiedad | Tipo de valor esperado |
| ------------- | ------------------- |
| computerManufacturer | cadena |
| operatingSystemSKU | número |
| operatingSystemVersion | version_string (p. ej.: "10.1. *") |
| productType | número |
| FQDNdeClúster | cadena |
| isHyperVRoleInstalled | booleano |
| isHyperVPowershellInstalled | booleano |
| isManagementToolsAvailable | booleano |
| isWmfInstalled | booleano |

Cada objeto de la matriz de inventario debe cumplir con la siguiente estructura de json:

``` json
"<property name>": {
    "type": "<expected type>",
    "operator": "<defined operator to use>",
    "value": "<expected value to evaluate using the operator>"
}
```

#### Valores de operador ####

| Operador | Descripción |
| -------- | ----------- |
| gt | mayor que |
| GE | mayor o igual a |
| lt | menor que |
| le | menor o igual a |
| EQ | igual a |
| ne | no es igual a |
|  esté  | comprobar si un valor es true |
| no | comprobar si un valor es false |
| contiene | existe un elemento en una cadena |
| notContains | elemento no existe en una cadena |

#### Tipos de datos ####

Opciones disponibles para la propiedad 'tipo':

| Tipo | Descripción |
| ---- | ----------- |
| version | un número de versión (p. ej.: 10.1. *) |
| número | un valor numérico |
| cadena | un valor de cadena |
| booleano | True o false |

#### Tipos de valor ####

La propiedad 'value' acepta estos tipos:

* cadena
* número
* booleano

Un conjunto de condición de inventario correctamente formada tiene este aspecto:

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

### Script ###

Por último, puedes ejecutar un script de PowerShell personalizado para identificar la disponibilidad y el estado del nodo. Todos los scripts deben devolver un objeto con la estructura siguiente:

``` ps
@{
    State = 'Available' | 'NotSupported' | 'NotConfigured';
    Message = '<Message to explain the reason of state such as not supported and not configured.>';
    Properties =
        @{ Name = 'Prop1'; Value = 'prop1 data'; Type = 'string' },
        @{Name='Prop2'; Value = 12345678; Type='number'; };
}
```
La propiedad de estado es el valor importante que controlará la decisión de mostrar u ocultar su extensión en la lista de herramientas.  Los valores permitidos son:
| Valor | Descripción |
| ---- | ----------- |
| Disponible | La extensión debe mostrarse en la lista de herramientas. |
| NotSupported | La extensión no debe mostrarse en la lista de herramientas. |
| No configurado | Se trata de un valor de marcador de posición para el trabajo futuras que solicitará al usuario para la configuración adicional antes de que la herramienta se pone a disposición.  Actualmente, este valor dará como resultado la herramienta que se muestra y es el equivalente funcional a 'Disponible'. |

Por ejemplo, si queremos que una herramienta para cargar solo si el servidor remoto tiene instalado BitLocker, la secuencia de comandos tiene este aspecto:

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

Una configuración de punto de entrada con la opción de script tiene este aspecto:

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

## Compatibilidad con varios conjuntos de requisito ##

Puedes usar más de un conjunto de requisitos para determinar cuándo se deben mostrar tu herramienta mediante la definición de varios bloques "requisitos".

Por ejemplo, para mostrar la herramienta si "scenario A" o "scenario B" es true, define dos bloques de requisitos; Si bien es true (es decir, se cumplan todas las condiciones dentro de un bloque de requisitos), se muestra la herramienta.

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

## Compatibilidad con los intervalos de condición ##

También puedes definir una variedad de condiciones mediante la definición de varios bloques "condiciones" con la misma propiedad, pero con distintos operadores.

Cuando la misma propiedad se define con distintos operadores, la herramienta se muestra siempre que sea el valor entre las dos condiciones.

Por ejemplo, esta herramienta se muestra siempre que el sistema operativo es una versión entre 6.3.0 y 10.0.0:

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
