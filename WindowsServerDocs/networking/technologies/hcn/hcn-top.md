---
title: API de servicio de Compute Network (HCN) para máquinas virtuales y contenedores
description: La API de servicio de proceso de host (HCN) es una API de Win32 orientada al público que proporciona acceso de nivel de plataforma para administrar las redes virtuales, los puntos de conexión de la red virtual y las directivas asociadas. Juntos, proporciona conectividad y seguridad para máquinas virtuales (VM) y contenedores que se ejecutan en un host de Windows.
ms.author: daschott
author: daschott
ms.date: 11/05/2018
ms.topic: article
ms.openlocfilehash: 48e7492809e7ef83faea32824cc816cec83ed955
ms.sourcegitcommit: 40905b1f9d68f1b7d821e05cab2d35e9b425e38d
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/06/2021
ms.locfileid: "97947531"
---
# <a name="host-compute-network-hcn-service-api-for-vms-and-containers"></a>API de servicio de Compute Network (HCN) para máquinas virtuales y contenedores

> Se aplica a: Windows Server (canal semianual), Windows Server 2019

La API de servicio de proceso de host (HCN) es una API de Win32 orientada al público que proporciona acceso de nivel de plataforma para administrar las redes virtuales, los puntos de conexión de la red virtual y las directivas asociadas. Juntos, proporciona conectividad y seguridad para máquinas virtuales (VM) y contenedores que se ejecutan en un host de Windows.

Los desarrolladores usan la API del servicio HCN para administrar las redes de las máquinas virtuales y los contenedores en sus flujos de trabajo de la aplicación. La API de HCN se ha diseñado para proporcionar la mejor experiencia para los desarrolladores. Los usuarios finales no interactúan directamente con estas API.

## <a name="features-of-the-hcn-service-api"></a>Características de la API del servicio HCN
-    Se implementa como una API de C hospedada por el servicio de red de host (SNP) en la máquina virtual o en el núcleo.

-    Proporciona la capacidad de crear, modificar, eliminar y enumerar objetos HCN como redes, extremos, espacios de nombres y directivas. Las operaciones se realizan en los identificadores de los objetos (por ejemplo, un identificador de red) y, internamente, estos identificadores se implementan mediante identificadores de contexto de RPC.

-    Basado en esquema. La mayoría de las funciones de la API definen parámetros de entrada y salida como cadenas que contienen los argumentos de la llamada de función como documentos JSON. Los documentos JSON se basan en esquemas fuertemente tipados y con control de versiones; estos esquemas forman parte de la documentación pública.

-    Se proporciona una API de suscripción/devolución de llamada para permitir que los clientes se registren para notificaciones de eventos de todo el servicio, como creaciones y eliminaciones de red.

-    La API de HCN funciona en Bridge Desktop (también conocido como Centennial) que se ejecutan en servicios del sistema. La API comprueba la ACL recuperando el token de usuario del autor de la llamada.

>[!TIP]
>La API del servicio HCN se admite en las tareas en segundo plano y en las ventanas que no son de primer plano.
## <a name="terminology-host-vs-compute"></a>Terminología: host frente a proceso
El servicio de proceso de host permite a los autores de llamadas crear y administrar máquinas virtuales y contenedores en un solo equipo físico. Se denomina para seguir la terminología del sector.

- El **host** se utiliza ampliamente en el sector de virtualización para hacer referencia al sistema operativo que proporciona recursos virtualizados.

- **Compute** se usa para hacer referencia a métodos de virtualización que son más amplios que las máquinas virtuales. Host Compute Network Service permite a los autores de llamadas crear y administrar redes para máquinas virtuales y contenedores en un solo equipo físico.

## <a name="schema-based-configuration-documents"></a>Documentos de configuración basados en esquemas
Los documentos de configuración basados en esquemas bien definidos son un estándar del sector establecido en el espacio de virtualización. La mayoría de las soluciones de virtualización, como Docker y Kubernetes, proporcionan API basadas en documentos de configuración. Varias iniciativas del sector, con la participación de Microsoft, impulsan un ecosistema para definir y validar estos esquemas, como [OpenAPI](https://www.openapis.org/).  Estas iniciativas también controlan la normalización de definiciones de esquema específicas para los esquemas que se usan para los contenedores, como [Open Container Initiative (OCI)](https://www.opencontainers.org/).

El idioma que se usa para crear documentos de configuración es [JSON](https://tools.ietf.org/html/rfc8259), que se usa en combinación con:
-    Definiciones de esquema que definen un modelo de objetos para el documento
-    Validación de si un documento JSON se ajusta a un esquema
-    Conversión automatizada de documentos JSON a y desde representaciones nativas de estos esquemas en los lenguajes de programación usados por los autores de llamadas de las API

Las definiciones de esquema usadas con frecuencia son [OpenAPI](https://www.openapis.org/) y el [esquema JSON](http://json-schema.org/), que le permite especificar las definiciones detalladas de las propiedades de un documento, por ejemplo:
-    Conjunto válido de valores para una propiedad, como 0-100 para una propiedad que representa un porcentaje.
-    La definición de las enumeraciones, que se representan como un conjunto de cadenas válidas para una propiedad.
-    Expresión regular para el formato esperado de una cadena.

Como parte de la documentación de las API de HCN, tenemos previsto publicar el esquema de nuestros documentos JSON como una especificación de OpenAPI. En función de esta especificación, las representaciones específicas del lenguaje del esquema pueden permitir el uso con seguridad de tipos de los objetos de esquema en el lenguaje de programación utilizado por el cliente.

### <a name="example"></a>Ejemplo

A continuación se presenta un ejemplo de este flujo de trabajo para el objeto que representa una controladora SCSI en el documento de configuración de una máquina virtual.

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
>Las anotaciones [NewIn ("2.0") forman parte de la compatibilidad de control de versiones de las definiciones de esquema.
A partir de esta definición interna, se generan las especificaciones de OpenAPI para el esquema:

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

Puede usar herramientas, como [Swagger](https://swagger.io/), para generar representaciones específicas del lenguaje del lenguaje de programación de esquemas que usa un cliente. Swagger admite una gran variedad de lenguajes, como C#, Go, JavaScript y Python.

- [Ejemplo de código C# generado](example-c-sharp.md) para el objeto de subred & IPAM de nivel superior.

- [Ejemplo de código de go generado](example-go.md) para el objeto de subred de & de IPAM de nivel superior. Docker usa Go y Kubernetes, que son dos de los consumidores de las API del servicio de red de proceso de host. Go tiene compatibilidad integrada para la serialización de tipos Go a y desde documentos JSON.

Además de la generación y validación de código, puede usar herramientas para simplificar el trabajo con documentos JSON, es decir, [Visual Studio Code](https://code.visualstudio.com/Docs/languages/json).

### <a name="top-level-objects-defined-in-the-hcnschemasmars-file"></a>Objetos de nivel superior definidos en HCN. Archivo schemas. Mars
Como se mencionó anteriormente, puede encontrar el esquema de documento para los documentos usados por las API de HCN en un conjunto de archivos. Mars en: onecore/VM/DV/net/SNP/Schema/Mars/Schema.

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

- Obtenga más información sobre los [escenarios de HCN comunes](hcn-scenarios.md).

- Obtenga más información sobre los [identificadores de contexto RPC para HCN](hcn-declaration-handles.md).

- Más información sobre los [esquemas de documento JSON de HCN](hcn-json-document-schemas.md).