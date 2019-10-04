---
title: Cambios en el tipo de conexión del clúster en el centro de administración de Windows v1909
description: Cambios en el tipo de conexión del clúster en el centro de administración de Windows v1909
ms.technology: manage
ms.topic: article
author: daniellee-msft
ms.author: jol
ms.date: 10/01/2019
ms.localizationpriority: medium
ms.prod: windows-server-threshold
ms.openlocfilehash: a07b30517f0d45b7e6f4f41f0ef9a6549e6e2117
ms.sourcegitcommit: de71970be7d81b95610a0977c12d456c3917c331
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/04/2019
ms.locfileid: "71952776"
---
# <a name="cluster-connection-type-changes-in-windows-admin-center-v1909"></a>Cambios en el tipo de conexión del clúster en el centro de administración de Windows v1909

>Se aplica a: Windows Admin Center, versión preliminar de Windows Admin Center

> [!IMPORTANT]
> En este documento se describen los cambios necesarios para que los desarrolladores de extensiones del centro de administración de Windows desarrollen herramientas del centro de administración de Windows para el clúster de conmutación por error y las soluciones de clúster hiperconvergido (HCl). Se trata de un cambio obligatorio necesario para que la extensión sea compatible con la versión preliminar del centro de administración de Windows v1909 y futuras versiones de disponibilidad general.

En el centro de administración de Windows v1909, hemos unificado los dos tipos diferentes de conexión de clúster (clúster de conmutación por error y conexiones de clúster hiperconvergido) en un solo tipo de conexión de clúster. Los usuarios ya no tendrán que identificar la configuración de un clúster con anterioridad para decidir en qué tipo de conexión debe agregarse el clúster, ni agregar el clúster dos veces como distintos tipos de conexión para obtener acceso a los diferentes conjuntos de herramientas. Los clústeres ahora se pueden agregar como "clúster de Windows Server" y se cargarán las herramientas adecuadas, principalmente en función de si Espacios de almacenamiento directo está habilitado o no.

Dado que esto requirió un cambio en la definición del tipo de conexión y cómo las herramientas relacionadas con el clúster deciden cuándo cargarse, las extensiones que proporcionan herramientas para los clústeres (clústeres de HCl o no HCI, o ambas) requerirán los cambios en la implementación, tal como se describe. menor.

## <a name="manifestjson---solutionsids-and-connectiontypes"></a>manifest. JSON: solutionsIds y connectionTypes

Anteriormente, para que se muestre la herramienta para un clúster de conmutación por error o un tipo de conexión de clúster de HCI, habría usado una de las siguientes definiciones en el archivo ```manifest.json```.

Para los clústeres de conmutación por error:
``` json
    {
        "entryPointType": "tool",
        "name": "MyToolName",
        "urlName": "MyToolUrl",
        "displayName": "MyToolDisplayName",
        "description": "MyToolDescription",
        "icon": "MyToolIcon",
        "path": "MyToolPath",
        "iframeId": "MyToolIframeId",
        "requirements": [
        {
            "solutionIds": [
                "msft.sme.failover-cluster!failover-cluster"
            ],
            "connectionTypes": [
                "msft.sme.connection-type.cluster"
            ]
        }
        ]
    }
```

En el caso de los clústeres HCI, la sección resaltada anterior se habría reemplazado por lo siguiente:
``` json
    "requirements": [
    {
        "solutionIds": [
            "msft.sme.software-defined-data-center!SDDC"
        ],
        "connectionTypes": [
            "msft.sme.connection-type.hyper-converged-cluster"
        ]
    }
    ]
```

En el centro de administración de Windows 1909 y versiones posteriores, los dos solutionIds y connectionTypes se han reemplazado por lo siguiente:
``` json
    "requirements": [
    {
        "solutionIds": [
            "msft.sme.software-defined-data-center!cluster"
        ],
        "connectionTypes": [
            "msft.sme.connection-type.cluster"
        ]
    }
    ]
```

Este es el único tipo de solutionIds y connectionTypes relacionado con el clúster admitido desde ahora. Si la herramienta solo se define con este tipo de solutionIds y connectionTypes, se cargará para cualquier conexión de clúster de conmutación por error, independientemente de si se trata de un clúster de HCI o no. Si desea limitar la herramienta para que solo esté disponible para los clústeres de HCI o los clústeres que no sean de HCI, deberá utilizar además las nuevas propiedades de inventario que se describen en la sección siguiente.

## <a name="manifestjson--inventory-properties"></a>manifest. JSON: propiedades de inventario
Al conectarse a un servidor o un clúster, el centro de administración de Windows consultará un conjunto de propiedades de inventario que puede usar para compilar condiciones con el fin de determinar cuándo debe estar disponible la herramienta o no (consulte la sección "propiedades de inventario" en el control de la [herramienta ](dynamic-tool-display.md)documento de visibilidad para obtener más información). En el centro de administración de Windows v1909, hemos agregado dos nuevas propiedades a esta lista que se pueden usar para determinar si un clúster es un clúster hiperconvergido o no. 

### <a name="iss2denabled"></a>isS2dEnabled
Técnicamente, un clúster hiperconvergido se define como un clúster de conmutación por error con Espacios de almacenamiento directo (S2D) habilitado. Si desea que la herramienta solo esté disponible para los clústeres hiperconvergidos, es decir, cuando S2D está habilitado, agregue la siguiente condición de inventario:
``` json
    "requirements": [
    {
        "solutionIds": [
            "msft.sme.software-defined-data-center!cluster"
        ],
        "connectionTypes": [
            "msft.sme.connection-type.cluster"
        ],
        "conditions": [
        {
            "inventory": {
                "isS2dEnabled": {
                    "type": "boolean",
                    "operator": "is"
                }
            }
        }
        ]
    }
    ]
```
> [!TIP] 
> Si desea que la herramienta esté disponible solo cuando S2D no está habilitado, cambie el valor de "operador" a "no".

### <a name="isbritannicaenabled"></a>isBritannicaEnabled
Además, si depende del recurso del clúster de administración de SDDC y usa el modelo de objetos de SDDC, puede comprobar la siguiente condición:
``` json
    "conditions": [
        {
            "inventory": {
                "isS2dEnabled": {
                    "type": "boolean",
                    "operator": "is"
                },
                "isBritannicaEnabled": {
                    "type": "boolean",
                    "operator": "is"
                }
            }
        }
    ]
```

## <a name="backward-compatibility-to-support-previous-versions-of-windows-admin-center"></a>Compatibilidad con versiones anteriores del centro de administración de Windows
Para asegurarse de que la extensión sigue funcionando con versiones anteriores del centro de administración de Windows, como la versión de disponibilidad general de v1904, la definición anterior de solutionIds y connectionTypes se puede usar junto con la nueva definición. Vea el ejemplo siguiente para mostrar la herramienta solo para los clústeres de HCI en todas las versiones del centro de administración de Windows.
``` json
    "requirements": [
    {
        "solutionIds": [
            "msft.sme.software-defined-data-center!SDDC"
        ],
        "connectionTypes": [
            "msft.sme.connection-type.hyper-converged-cluster"
        ]
    },
    {
        "solutionIds": [
            "msft.sme.software-defined-data-center!SDDC",
            "msft.sme.software-defined-data-center!cluster"
        ],
        "connectionTypes": [
            "msft.sme.connection-type.hyper-converged-cluster",
            "msft.sme.connection-type.cluster"
        ],
        "conditions": [
            {
                "inventory": {
                    "isS2dEnabled": {
                        "type": "boolean",
                        "operator": "is"
                    }
                }
            }
        ]
    }
    ]
```

## <a name="known-issue-appcontextserviceactiveconnectionishyperconvergedclusterisfailovercluster-is-not-set-properly-in-windows-admin-center-v1909"></a>Problema conocido: AppContextService. activeConnection. isHyperConvergedCluster/isFailoverCluster no se ha establecido correctamente en el centro de administración de Windows v1909
Una regresión de los cambios recientes es que las propiedades ```AppContextService.activeConnection.isHyperConvergedCluster/isFailoverCluster``` no se establecen correctamente en el centro de administración de Windows v1909 y siempre serán false. Esto se corregirá en la siguiente versión, V1910, pero también estará en desuso y dejará de estar disponible en la siguiente versión de GA en 2020. En el futuro, puede reemplazarlo por el código siguiente y usar ```this.connectHCI```.
```
    import { ClusterInventoryCache } from '@msft-sme/core';

    constructor(
        ...
        private appContextService: AppContextService,
        ...
    ) {
        ...
        this.clusterInventoryCache = new ClusterInventoryCache(
            this.appContextService,
            <SharedCacheOptions>{ expiration: Constants.sharedCacheExpiration }
        );

         this.isBritannicEnabled().subscribe(result => this.connectHCI = result);
    }

    private isBritannicEnabled(): Observable<boolean> {
        return this.clusterInventoryCache.query(this.getClusterInventoryQueryParameters())
        .pipe(
            map(inventory => {
                return inventory.instance.isBritannicaEnabled;
        }));
    }

    private getClusterInventoryQueryParameters(): ClusterInventoryParams {
        const nodeName = this.appContextService.activeConnection.validNodeName;
        const endpoint = this.appContextService.authorizationManager.getJeaEndpoint(nodeName);
        const options = { powerShellEndpoint: endpoint };
        return {
            name: nodeName,
            requestOptions: options
        };
    }
```