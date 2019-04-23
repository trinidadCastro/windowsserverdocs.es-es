---
title: Hospedar las API del servicio de proceso red ((HCN)) para máquinas virtuales y contenedores
description: Host de API del servicio de proceso red ((HCN)) es una API de Win32 orientados al público que proporciona acceso de nivel de plataforma para administrar las redes virtuales, los puntos de conexión de red virtual y las directivas asociadas. Juntos Esto proporciona conectividad y seguridad para máquinas virtuales (VM) y contenedores que se ejecutan en un host de Windows.
ms.author: jmesser
author: jmesser81
ms.date: 11/05/2018
ms.openlocfilehash: 50af0dab69633aa6e07ded68e9246aa0315377f0
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59844986"
---
# <a name="host-compute-network-hcn-service-api-for-vms-and-containers"></a>Hospedar las API del servicio de proceso red ((HCN)) para máquinas virtuales y contenedores

>Se aplica a: Windows Server (canal semianual), Windows Server 2016

Host de API del servicio de proceso red ((HCN)) es una API de Win32 orientados al público que proporciona acceso de nivel de plataforma para administrar las redes virtuales, los puntos de conexión de red virtual y las directivas asociadas. Juntos Esto proporciona conectividad y seguridad para máquinas virtuales (VM) y contenedores que se ejecutan en un host de Windows. 

Los desarrolladores usar la API del servicio (HCN) para administrar redes para máquinas virtuales y contenedores en sus flujos de trabajo de aplicación. La API (HCN) se ha diseñado para ofrecer la mejor experiencia para desarrolladores. Los usuarios finales no interactúan directamente con estas API.  

## <a name="features-of-the-hcn-service-api"></a>Características de la API de servicio (HCN)
-   Se implementa como API de C hospedada por el servicio de red de Host (HNS) OnCore o máquina virtual.

-   Proporciona la capacidad de crear, modificar, eliminar y enumerar los objetos (HCN) como redes, los puntos de conexión, los espacios de nombres y las directivas. Realizarán operaciones en los identificadores de los objetos (por ejemplo, un identificador de red) e internamente estos identificadores se implementan mediante identificadores de contexto RPC.

-   Basado en el esquema. Mayoría de las funciones de la API de define la entrada y los parámetros como cadenas que contiene los argumentos de la llamada de función como documentos JSON de salida. Los documentos JSON se basan en los esquemas fuertemente tipados y con control de versiones, estos esquemas son parte de la documentación pública. 

-   Una suscripción o devolución de llamada de API se proporciona para permitir que los clientes registrar las notificaciones de eventos de todo el servicio, como red de la creación y eliminación.

-   API (HCN) funciona en Desktop Bridge (conocido como) Aplicaciones de Centennial) que se ejecutan en los servicios del sistema. La API comprueba la ACL al recuperar el token de usuario desde el llamador.

>[!TIP]
>La API del servicio (HCN) se admite en windows no aparecen en primer plano y de tareas en segundo plano. 

## <a name="terminology-host-vs-compute"></a>Terminología: Host vs. Cálculo
El servicio de proceso de host permite que los llamadores crear y administrar máquinas virtuales y contenedores en un único equipo físico. Se llama a seguir la terminología de la industria. 

- **Host** se usa ampliamente en la industria de la virtualización para hacer referencia al sistema operativo que proporciona recursos virtualizados.

- **Proceso** se usa para referirse a los métodos de virtualización más amplios que solo máquinas virtuales. Servicio de red de host de proceso permite que los llamadores crear y administrar las redes para máquinas virtuales y contenedores en un único equipo físico.

## <a name="schema-based-configuration-documents"></a>Documentos de configuración basadas en esquema
Documentos de configuración basadas en esquemas bien definidos es un estándar del sector establecido en el espacio de virtualización. La mayoría de las soluciones de virtualización, como Docker y Kubernetes, se proporcionan que las API basadas en documentos de configuración. Varias iniciativas del sector, con la participación de Microsoft, un ecosistema para definir y validar estos esquemas, como la unidad [OpenAPI](https://www.openapis.org/).  Estas iniciativas unidad también la estandarización de las definiciones de esquema específico para los esquemas utilizados para los contenedores, como [Open Container Initiative (OCI)](https://www.opencontainers.org/).

El idioma utilizado para la creación de documentos de configuración es [JSON](https://tools.ietf.org/html/rfc8259), que se utiliza en combinación con:
-   Definiciones de esquema que definen un modelo de objetos para el documento
-   Validación de si un documento JSON se ajusta a un esquema
-   Automatizar la conversión de documentos JSON hacia y desde representaciones nativas de estos esquemas en los lenguajes de programación utilizados por los autores de llamadas de API 

Las definiciones de esquema utilizadas con frecuencia son [OpenAPI](https://www.openapis.org/) y [esquema JSON](http://json-schema.org/), que le permite especificar las definiciones detalladas de las propiedades de un documento, por ejemplo:
-   El conjunto válido de valores para una propiedad, como 0 y 100 para una propiedad que representa un porcentaje.
-   La definición de enumeraciones, que se representan como un conjunto de cadenas válidas para una propiedad.
-   Una expresión regular para el formato esperado de una cadena. 

Como parte de la documentación de las API (HCN), tenemos previsto publicar el esquema de los documentos JSON como una especificación OpenAPI. Según esta especificación, pueden permitir representaciones de lenguaje específico del esquema para su uso con seguridad de tipos de los objetos de esquema en el lenguaje de programación utilizado por el cliente. 

### <a name="example"></a>Ejemplo 

El siguiente es un ejemplo de este flujo de trabajo para el objeto que representa una controladora SCSI en el documento de configuración de una máquina virtual. 

En el código fuente de Windows, definimos los esquemas mediante archivos .mars: onecore/vm/dv/net/hns/schema/mars/Schema/HCN.Schema.Network.mars

```
enum IpamType
{
    [NewIn("2.0")] Static,
    [NewIn("2.0")] Dhcp,
};

class Ipam
{
    // Type : dhcp
    [NewIn("2.0"),OmitEmpty] IpamType   Type;
    [NewIn("2.0"),OmitEmpty] Subnet     Subnets[];
};

class Subnet : HCN.Schema.Common.Base
{
    [NewIn("2.0"),OmitEmpty] string         IpAddressPrefix;
    [NewIn("2.0"),OmitEmpty] SubnetPolicy   Policies[];
    [NewIn("2.0"),OmitEmpty] Route          Routes[];
};


enum SubnetPolicyType
{
    [NewIn("2.0")] VLAN
};

class SubnetPolicy
{
    [NewIn("2.0"),OmitEmpty] SubnetPolicyType                 Type;
    [NewIn("2.0"),OmitEmpty] HCN.Schema.Common.PolicySettings Data;
};

class PolicySettings
{
    [NewIn("2.0"),OmitEmpty]  string      Name;
};

class VlanPolicy : HCN.Schema.Common.PolicySettings 
{
    [NewIn("2.0")] uint32 IsolationId;
};

class Route 
{
    [NewIn("2.0"),OmitEmpty] string NextHop;
    [NewIn("2.0"),OmitEmpty] string DestinationPrefix;
    [NewIn("2.0"),OmitEmpty] uint16 Metric;
};

```

>[!TIP]
>El [NewIn("2.0") anotaciones forman parte de la compatibilidad de versiones de las definiciones de esquema.

De esta definición interna, generamos las especificaciones de OpenAPI para el esquema:

```
{ 
    "swagger" : "2.0", 
    "info" : { 
       "version" : "2.1", 
       "title" : "HCN API" 
    },
    "definitions": {        
        "Ipam": {
            "type": "object",
            "properties": {
                "Type": {
                    "type": "string",
                    "enum": [
                        "Static",
                        "Dhcp"
                    ],
                    "description": " Type : dhcp"
                },
                "Subnets": {
                    "type": "array",
                    "items": {
                        "$ref": "#/definitions/Subnet"
                    }
                }
            }
        },
        "Subnet": {
            "type": "object",
            "properties": {
                "ID": {
                    "type": "string",
                    "pattern": "^[0-9A-Fa-f]{8}-([0-9A-Fa-f]{4}-){3}[0-9A-Fa-f]{12}$"
                },                
                "IpAddressPrefix": {
                    "type": "string"
                },
                "Policies": {
                    "type": "array",
                    "items": {
                        "$ref": "#/definitions/SubnetPolicy"
                    }
                },
                "Routes": {
                    "type": "array",
                    "items": {
                        "$ref": "#/definitions/Route"
                    }
                }
            }
        },
        "SubnetPolicy": {
            "type": "object",
            "properties": {
                "Type": {
                    "type": "string",
                    "enum": [
                        "VLAN",
                        "VSID"
                    ]
                },
                "Data": {
                    "$ref": "#/definitions/PolicySettings"
                }
            }
        },
        "PolicySettings": {
            "type": "object",
            "properties": {
                "Name": {
                    "type": "string"
                }
            }
        },                      
        "VlanPolicy": {
            "type": "object",
            "properties": {
                "Name": {
                    "type": "string"
                },
                "IsolationId": {
                    "type": "integer",
                    "format": "uint32"
                }
            }
        },
        "Route": {
            "type": "object",
            "properties": {
                "NextHop": {
                    "type": "string"
                },
                "DestinationPrefix": {
                    "type": "string"
                },
                "Metric": {
                    "type": "integer",
                    "format": "uint16"
                }
            }
        }        
    }
} 
```

Puede usar las herramientas, como [Swagger](https://swagger.io/), para generar representaciones de lenguaje específico del esquema de lenguaje de programación utilizado por un cliente. Swagger es compatible con una variedad de lenguajes, como C#, Go, Javascript y Python).

- [Ejemplo de generado C# código](example-c-sharp.md) subred & IPAM de nivel superior de objetos.

- [Ejemplo de código de Go generado](example-go.md) subred & IPAM de nivel superior de objetos. Go está usando Docker y Kubernetes, que son dos de los consumidores de las API de servicio de red de proceso de Host. Go tiene compatibilidad integrada para la serialización de tipos de ir a y desde documentos JSON.

Además de la validación y generación de código, puede usar herramientas para simplificar el trabajo con documentos JSON, es decir, [Visual Studio Code](https://code.visualstudio.com/Docs/languages/json).

### <a name="top-level-objects-defined-in-the-hcnschemasmars-file"></a>Objetos de nivel superior definidos en el (HCN). Archivo schemas.MARS
Como se mencionó anteriormente, puede encontrar el esquema de documento para documentos usados por las API (HCN) en un conjunto de archivos .mars bajo: onecore/vm/dv/net/SNP/esquema/mars/esquema

Los objetos de nivel superior son:
- [HostComputeNetwork](hcn-scenarios.md#scenario-hcn)
- [HostComputeEndpoint](hcn-scenarios.md#scenario-hcn-endpoint)
- [HostComputeNamespace](hcn-scenarios.md#scenario-hcn-namespace)
- [HostComputeLoadBalancer](hcn-scenarios.md#scenario-hcn-load-balancer)

```
class HostComputeNetwork : HCN.Schema.Common.Base
{
    [NewIn("2.0"),OmitEmpty] HCN.Schema.Network.NetworkMode          Type;
    [NewIn("2.0"),OmitEmpty] HCN.Schema.Network.NetworkPolicy        Policies[];
    [NewIn("2.0"),OmitEmpty] HCN.Schema.Network.MacPool              MacPool;
    [NewIn("2.0"),OmitEmpty] HCN.Schema.Network.DNS                  Dns;
    [NewIn("2.0"),OmitEmpty] HCN.Schema.Network.Ipam                 Ipams[];
};

class HostComputeEndpoint : HCN.Schema.Common.Base
{
    [NewIn("2.0"),OmitEmpty] string                                     HostComputeNetwork;
    [NewIn("2.0"),OmitEmpty] HCN.Schema.Network.Endpoint.EndpointPolicy Policies[];
    [NewIn("2.0"),OmitEmpty] HCN.Schema.Network.Endpoint.IpConfig       IpConfigurations[];
    [NewIn("2.0"),OmitEmpty] HCN.Schema.Network.DNS                     Dns;
    [NewIn("2.0"),OmitEmpty] HCN.Schema.Network.Route                   Routes[];
    [NewIn("2.0"),OmitEmpty] string                                     MacAddress;
};

class HostComputeNamespace : HCN.Schema.Common.Base
{
    [NewIn("2.0"),OmitEmpty] uint32                                    NamespaceId;
    [NewIn("2.0"),OmitEmpty] Guid                                      NamespaceGuid;
    [NewIn("2.0"),OmitEmpty] HCN.Schema.Namespace.NamespaceType        Type;
    [NewIn("2.0"),OmitEmpty] HCN.Schema.Namespace.NamespaceResource    Resources[];
};

class HostComputeLoadBalancer : HCN.Schema.Common.Base
{
    [NewIn("2.0"), OmitEmpty] string                                               HostComputeEndpoints[]; 
    [NewIn("2.0"), OmitEmpty] string                                               VirtualIPs[]; 
    [NewIn("2.0"), OmitEmpty] HCN.Schema.Network.Endpoint.Policy.PortMappingPolicy PortMappings[]; 
    [NewIn("2.0"), OmitEmpty] HCN.Schema.LoadBalancer.LoadBalancerPolicy           Policies[];
};
```

## <a name="next-steps"></a>Pasos siguientes

- Obtenga más información sobre la [escenarios comunes de (HCN)](hcn-scenarios.md).

- Obtenga más información sobre la [manipuladores del contexto RPC (HCN)](hcn-declaration-handles.md).

- Obtenga más información sobre la [esquemas de documentos JSON HCN](hcn-json-document-schemas.md).