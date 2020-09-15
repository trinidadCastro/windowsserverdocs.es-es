---
title: Escenarios de host de proceso de red (HCN)
description: Ejemplo que muestra cómo usar la API de servicio de red de compute de host para crear una red de proceso de host en el host que se puede usar para conectar las NIC virtuales a Virtual Machines o contenedores.
ms.author: daschott
author: daschott
ms.date: 11/05/2018
ms.openlocfilehash: e865326b77fbdec19af3e7db347734715458b18a
ms.sourcegitcommit: 0b3d6661c44aa1a697087e644437279142726d84
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/15/2020
ms.locfileid: "90083686"
---
# <a name="common-scenarios"></a>Escenarios frecuentes

> Se aplica a: Windows Server (canal semianual), Windows Server 2019

## <a name="scenario-hcn"></a>Escenario: HCN

### <a name="create-an-hcn"></a>Creación de un HCN

En este ejemplo se muestra cómo usar la API de servicio de red de compute de host para crear una red de proceso de host en el host que se puede usar para conectar NIC virtuales a Virtual Machines o contenedores.

```C++
using unique_hcn_network = wil::unique_any<
    HCN_NETWORK,
    decltype(&HcnCloseNetwork),
    HcnCloseNetwork>;
/// Creates a simple HCN Network, waiting synchronously to finish the task
void CreateHcnNetwork()
{
    unique_hcn_network hcnnetwork;
    wil::unique_cotaskmem_string result;
    std::wstring settings = LR"(
    {
        "SchemaVersion": {
            "Major": 2,
            "Minor": 0
        },
        "Owner" : "WDAGNetwork",
        "Flags" : 0,
        "Type"  : 0,
        "Ipams" : [
            {
                "Type" : 0,
                "Subnets" : [
                    {
                        "IpAddressPrefix" : "192.168.1.0/24",
                        "Policies" : [
                            {
                                "Type" : "VLAN",
                                "IsolationId" : 100,
                            }
                        ],
                        "Routes" : [
                            {
                                "NextHop" : "192.168.1.1",
                                "DestinationPrefix" : "0.0.0.0/0",
                            }
                        ]
                    }
                ],
            },
        ],
        "MacPool":  {
            "Ranges" : [
                {
                    "EndMacAddress":  "00-15-5D-52-CF-FF",
                    "StartMacAddress":  "00-15-5D-52-C0-00"
                }
            ],
        },
        "Dns" : {
            "Suffix" : "net.home",
            "ServerList" : ["10.0.0.10"],
        }
    }
    })";
    GUID networkGuid;
    HRESULT result = CoCreateGuid(&networkGuid);
    result = HcnCreateNetwork(
        networkGuid,              // Unique ID
        settings.c_str(),      // Compute system settings document
        &hcnnetwork,
        &result
        );
    if (FAILED(result))
    {
                    // UnMarshal  the result Json
     // ErrorSchema
        //   {
        //    "ErrorCode" : <uint32>,
        //    "Error" : <string>,
        //    "Success" : <bool>,
       //   }
        // Failed to create network
        THROW_HR(result);
    }
    // Close the Handle
    result = HcnCloseNetwork(hcnnetwork.get());
    if (FAILED(result))
    {
        // UnMarshal  the result Json
        THROW_HR(result);
    }
}
```
### <a name="delete-an-hcn"></a>Eliminar un HCN
En este ejemplo se muestra cómo usar la API de servicio de red de compute de host para abrir & eliminar una red de proceso de host
```C++
    wil::unique_cotaskmem_string errorRecord;
    GUID networkGuid; // Initialize it to appropriate network guid value
    HRESULT hr = HcnDeleteNetwork(networkGuid, &errorRecord);
    if (FAILED(hr))
    {
        // UnMarshal the result Json
        THROW_HR(hr);
    }
```
### <a name="enumerate-all-networks"></a>Enumerar todas las redes
En este ejemplo se muestra cómo usar la API de servicio de red de compute de host para enumerar todas las redes de proceso del host.
```C++
     wil::unique_cotaskmem_string resultNetworks;
     wil::unique_cotaskmem_string errorRecord;
     // Filter to select Networks based on properties
     std::wstring filter [] = LR"(
     {
         "Name"  : "WDAG",
     })";
     HRESULT result = HcnEnumerateNetworks(filter.c_str(), &resultNetworks, &errorRecord);
     if (FAILED(result))
     {
         // UnMarshal  the result Json
         THROW_HR(result);
     }
```
### <a name="query-network-properties"></a>Consultar propiedades de red
En este ejemplo se muestra cómo usar la API de servicio de red de compute de host para consultar las propiedades de red.
```C++
    unique_hcn_network hcnnetwork;
    wil::unique_cotaskmem_string errorRecord;
    wil::unique_cotaskmem_string properties;
    std:wstring query = LR"(
    {
        // Future
    })";
    GUID networkGuid; // Initialize it to appropriate network guid value
    HRESULT hr = HcnOpenNetwork(networkGuid, &hcnnetwork, &errorRecord);
    if (FAILED(hr))
    {
        // UnMarshal  the result Json
        THROW_HR(hr);
    }
    hr = HcnQueryNetworkProperties(hcnnetwork.get(), query.c_str(), &properties, &errorRecord);
    if (FAILED(hr))
    {
        // UnMarshal  the result Json
        THROW_HR(hr);
    }
```
## <a name="scenario-hcn-endpoint"></a>Escenario: punto de conexión de HCN
### <a name="create-an-hcn-endpoint"></a>Creación de un punto de conexión de HCN
En este ejemplo se muestra cómo usar la API de servicio de red de compute de host para crear un punto de conexión de red de proceso de host y, a continuación, agregarlo en caliente a la máquina virtual o a un contenedor.
```C++
using unique_hcn_endpoint = wil::unique_any<
    HCN_ENDPOINT,
    decltype(&HcnCloseEndpoint),
    HcnCloseEndpoint>;
void CreateAndHotAddEndpoint()
{
    unique_hcn_endpoint hcnendpoint;
    unique_hcn_network hcnnetwork;
    wil::unique_cotaskmem_string errorRecord;
    std::wstring settings[] = LR"(
    {
        "SchemaVersion": {
            "Major": 2,
            "Minor": 0
        },
        "Owner" : "Sample",
                   "Flags" : 0,
        "HostComputeNetwork" : "87fdcf16-d210-426e-959d-2a1d4f41d6d3",
        "DNS" : {
            "Suffix" : "net.home",
            "ServerList" : "10.0.0.10",
        }
    })";
    GUID endpointGuid;
    HRESULT result = CoCreateGuid(&endpointGuid);
    result = HcnOpenNetwork(
        networkGuid,              // Unique ID
        &hcnnetwork,
        &errorRecord
        );
    if (FAILED(result))
    {
        // Failed to find network
        THROW_HR(result);
    }
    result = HcnCreateEndpoint(
        hcnnetwork.get(),
        endpointGuid,              // Unique ID
        settings.c_str(),      // Compute system settings document
        &hcnendpoint,
        &errorRecord
        );
    if (FAILED(result))
    {
        // Failed to create endpoint
        THROW_HR(result);
    }
    // Can use the sample from HCS API Spec on how to attach this endpoint
    // to the VM using AddNetworkAdapterToVm
    result = HcnCloseEndpoint(hcnendpoint.get());
    if (FAILED(result))
    {
        // UnMarshal  the result Json
        THROW_HR(result);
    }
}
```
### <a name="delete-an-endpoint"></a>Eliminar un extremo
En este ejemplo se muestra cómo usar la API de servicio de red de compute de host para eliminar un punto de conexión de red de proceso de host.
```C++
    wil::unique_cotaskmem_string errorRecord;
    GUID endpointGuid; // Initialize it to appropriate endpoint guid value
    HRESULT hr = HcnDeleteEndpoint(endpointGuid, &errorRecord);
    if (FAILED(hr))
    {
        // UnMarshal  the result Json
        THROW_HR(hr);
    }
```
### <a name="modify-and-endpoint"></a>Modificar y punto de conexión
En este ejemplo se muestra cómo usar la API de servicio de red de compute de host para modificar un punto de conexión de red de proceso host.
```C++
    unique_hcn_endpoint hcnendpoint;
    GUID endpointGuid; // Initialize it to appropriate endpoint guid value
    HRESULT hr = HcnOpenEndpoint(endpointGuid, &hcnendpoint, &errorRecord);
    if (FAILED(hr))
    {
        // UnMarshal  the result Json
        THROW_HR(hr);
    }
    std::wstring  ModifySettingAddPortJson = LR"(
    {
        "ResourceType" : 0,
        "RequestType" : 0,
        "Settings" : {
            "PortName" : "acbd341a-ec08-44c0-9d5e-61af0ee86902"
            "VirtualNicName" : "641313e1-7ae8-4ddb-94e5-3215f3a0b218--87fdcf16-d210-426e-959d-2a1d4f41d6d1"
            "VirtualMachineId" : "641313e1-7ae8-4ddb-94e5-3215f3a0b218"
        }
    }
    )";
    hr = HcnModifyEndpoint(hcnendpoint.get(), ModifySettingAddPortJson.c_str(), &errorRecord);
    if (FAILED(hr))
    {
        // UnMarshal  the result Json
        THROW_HR(hr);
    }
```
### <a name="enumerate-all-enpoints"></a>Enumerar todos los enpuntos
En este ejemplo se muestra cómo usar la API de servicio de red de compute de host para enumerar todos los puntos de conexión de red de proceso host.
```C++
    wil::unique_cotaskmem_string errorRecord;
    wil::unique_cotaskmem_string resultEndpoints;
    wil::unique_cotaskmem_string errorRecord;
    // Filter to select Endpoint based on properties
    std::wstring filter [] = LR"(
    {
        "Name"  : "sampleNetwork",
    })";
    result = HcnEnumerateEndpoints(filter.c_str(), &resultEndpoints, &errorRecord);
    if (FAILED(result))
    {
        THROW_HR(result);
    }
```
### <a name="query-endpoint-properties"></a>Propiedades de extremo de consulta
En este ejemplo se muestra cómo usar la API de servicio de red de compute de host para consultar todas las propiedades de un punto de conexión de red de proceso host.
```C++
    unique_hcn_endpoint hcnendpoint;
    wil::unique_cotaskmem_string errorRecord;
    GUID endpointGuid; // Initialize it to appropriate endpoint guid value
    HRESULT hr = HcnOpenEndpoint(endpointGuid, &hcnendpoint, &errorRecord);
    if (FAILED(hr))
    {
        // UnMarshal  the result Json
        THROW_HR(hr);
    }
    wil::unique_cotaskmem_string properties;
    std:wstring query = LR"(
    {
        // Future
    })";
    hr = HcnQueryEndpointProperties(hcnendpoint.get(), query.c_str(), &properties, &errorRecord);
    if (FAILED(hr))
    {
        // UnMarshal  the errorRecord Json
        THROW_HR(hr);
    }
```
## <a name="scenario-hcn-namespace"></a>Escenario: espacio de nombres HCN
### <a name="create-an-hcn-namespace"></a>Creación de un espacio de nombres HCN
En este ejemplo se muestra cómo usar la API de servicio de red de compute de host para crear un espacio de nombres de red de proceso de host en el host que se puede usar para conectar el punto de conexión y los contenedores.
```C++
using unique_hcn_namespace = wil::unique_any<
    HCN_NAMESPACE,
    decltype(&HcnCloseNamespace),
    HcnCloseNamespace>;
/// Creates a simple HCN Network, waiting synchronously to finish the task
void CreateHcnNamespace()
{
    unique_hcn_namespace handle;
    wil::unique_cotaskmem_string errorRecord;
    std::wstring settings = LR"(
    {
        "SchemaVersion": {
            "Major": 2,
            "Minor": 0
        },
        "Owner" : "Sample",
        "Flags" : 0,
        "Type" : 0,
    })";
    GUID namespaceGuid;
    HRESULT result = CoCreateGuid(&namespaceGuid);
    result = HcnCreateNamespace(
        namespaceGuid,              // Unique ID
        settings.c_str(),      // Compute system settings document
        &handle,
        &errorRecord
        );
    if (FAILED(result))
    {
                    // UnMarshal  the result Json
     // ErrorSchema
        //   {
        //    "ErrorCode" : <uint32>,
        //    "Error" : <string>,
        //    "Success" : <bool>,
       //   }
        // Failed to create network
        THROW_HR(result);
    }
    result = HcnCloseNamespace(handle.get());
    if (FAILED(result))
    {
        // UnMarshal  the result Json
        THROW_HR(result);
    }
}
```
### <a name="delete-an-hcn-namespace"></a>Eliminación de un espacio de nombres HCN
En este ejemplo se muestra cómo usar la API de servicio de red de compute de host para eliminar un espacio de nombres de red de proceso de host.
```C++
    wil::unique_cotaskmem_string errorRecord;
    GUID namespaceGuid; // Initialize it to appropriate namespace guid value
    HRESULT hr = HcnDeleteNamespace(namespaceGuid, &errorRecord);
    if (FAILED(hr))
    {
        // UnMarshal the result Json
        THROW_HR(hr);
    }
```
### <a name="modify-an-hcn-namespace"></a>Modificar un espacio de nombres HCN
En este ejemplo se muestra cómo usar la API de servicio de red de compute de host para modificar un espacio de nombres de red de proceso host.
```C++
    unique_hcn_namespace handle;
    GUID namespaceGuid; // Initialize it to appropriate namespace guid value
    HRESULT hr = HcnOpenNamespace(namespaceGuid, &handle, &errorRecord);
    if (FAILED(hr))
    {
        // UnMarshal  the result Json
        THROW_HR(hr);
    }
    wil::unique_cotaskmem_string errorRecord;
    static std::wstring  ModifySettingAddEndpointJson = LR"(
    {
        "ResourceType" : 1,
        "ResourceType" : 0,
        "Settings" : {
            "EndpointId" : "87fdcf16-d210-426e-959d-2a1d4f41d6d1"
        }
    }
    )";
    hr = HcnModifyNamespace(handle.get(), ModifySettingAddEndpointJson.c_str(), &errorRecord);
    if (FAILED(hr))
    {
        // UnMarshal the result Json
        THROW_HR(hr);
    }
    hr = HcnCloseNamespace(handle.get());
    if (FAILED(hr))
    {
        // UnMarshal  the result Json
        THROW_HR(hr);
    }
```
### <a name="enumerate-all-namespaces"></a>Enumerar todos los espacios de nombres
En este ejemplo se muestra cómo usar la API de servicio de red de compute de host para enumerar todos los espacios de nombres de red de proceso host.
```C++
    wil::unique_cotaskmem_string resultNamespaces;
    wil::unique_cotaskmem_string errorRecord;
    std::wstring filter [] = LR"(
    {
            // Future
    })";
    HRESULT hr = HcnEnumerateNamespace(filter.c_str(), &resultNamespaces, &errorRecord);
    if (FAILED(hr))
    {
        // UnMarshal  the result Json
        THROW_HR(hr);
    }
```
### <a name="query-namespace-properties"></a>Propiedades del espacio de nombres de consulta
En este ejemplo se muestra cómo usar la API de servicio de red de compute de host para consultar las propiedades de espacio de nombres de red de proceso host.
```C++
    unique_hcn_namespace handle;
    GUID namespaceGuid; // Initialize it to appropriate namespace guid value
    HRESULT hr = HcnOpenNamespace(namespaceGuid, &handle, &errorRecord);
    if (FAILED(hr))
    {
        // UnMarshal  the result Json
        THROW_HR(hr);
    }
    wil::unique_cotaskmem_string errorRecord;
    wil::unique_cotaskmem_string properties;
    std:wstring query = LR"(
    {
        // Future
    })";
    HRESULT hr = HcnQueryNamespaceProperties(handle.get(), query.c_str(), &properties, &errorRecord);
    if (FAILED(hr))
    {
        // UnMarshal  the result Json
        THROW_HR(hr);
    }
```
## <a name="scenario-hcn-load-balancer"></a>Escenario: HCN load balancer
### <a name="create-an-hcn-load-balancer"></a>Creación de un equilibrador de carga de HCN
En este ejemplo se muestra cómo usar la API de servicio de red de compute de host para crear una red de proceso de host Load Balancer en el host que se puede usar para equilibrar la carga del extremo en el proceso.
```C++
using unique_hcn_loadbalancer = wil::unique_any<
    HCN_LOADBALANCER,
    decltype(&HcnCloseLoadBalancer),
    HcnCloseLoadBalancer>;
/// Creates a simple HCN LoadBalancer, waiting synchronously to finish the task
void CreateHcnLoadBalancer()
{
    unique_hcn_loadbalancer handle;
    wil::unique_cotaskmem_string errorRecord;
    std::wstring settings = LR"(
     {
        "SchemaVersion": {
            "Major": 2,
            "Minor": 0
        },
        "Owner" : "Sample",
        "HostComputeEndpoints" : [
            "87fdcf16-d210-426e-959d-2a1d4f41d6d1"
        ],
        "VirtualIPs" : [ "10.0.0.10" ],
        "PortMappings" : [
            {
                "Protocol" : 0,
                "InternalPort" : 8080,
                "ExternalPort" : 80,
            }
        ],
        "EnableDirectServerReturn" : true,
        "InternalLoadBalancer" : false,
    }
     )";
    GUID lbGuid;
    HRESULT result = CoCreateGuid(&lbGuid);
    HRESULT hr = HcnCreateLoadBalancer(
        lbGuid,              // Unique ID
        settings.c_str(),      // LoadBalancer settings document
        &handle,
        &errorRecord
        );
    if (FAILED(hr))
    {
                    // UnMarshal  the result Json
     // ErrorSchema
        //   {
        //    "ErrorCode" : <uint32>,
        //    "Error" : <string>,
        //    "Success" : <bool>,
       //   }
        // Failed to create network
        THROW_HR(hr);
    }
    hr = HcnCloseLoadBalancer(handle.get());
    if (FAILED(hr))
    {
        // UnMarshal  the result Json
        THROW_HR(hr);
    }
}
```
### <a name="delete-an-hcn-load-balancer"></a>Eliminación de un equilibrador de carga de HCN
En este ejemplo se muestra cómo usar la API de servicio de red de proceso de host para eliminar una red de proceso de host LoadBalancer.
```C++
    wil::unique_cotaskmem_string errorRecord;
    GUID lbGuid; // Initialize it to appropriate loadbalancer guid value
    HRESULT hr = HcnDeleteLoadBalancer(lbGuid , &errorRecord);
    if (FAILED(hr))
    {
        // UnMarshal the result Json
        THROW_HR(hr);
    }
```
### <a name="modify-an-hcn-load-balancer"></a>Modificación de un equilibrador de carga de HCN
En este ejemplo se muestra cómo usar la API de servicio de red de compute de host para modificar un espacio de nombres de red de proceso host.
```C++
    unique_hcn_loadbalancer handle;
    GUID lbGuid; // Initialize it to appropriate loadbalancer guid value
    HRESULT hr = HcnOpenLoadBalancer(lbGuid, &handle, &errorRecord);
    if (FAILED(hr))
    {
        // UnMarshal  the result Json
        THROW_HR(hr);
    }
    wil::unique_cotaskmem_string errorRecord;
    static std::wstring  ModifySettingAddEndpointJson = LR"(
    {
        "ResourceType" : 1,
        "ResourceType" : 0,
        "Settings" : {
            "EndpointId" : "87fdcf16-d210-426e-959d-2a1d4f41d6d1"
        }
    }
    )";
    hr = HcnModifyLoadBalancer(handle.get(), ModifySettingAddEndpointJson.c_str(), &errorRecord);
    if (FAILED(hr))
    {
        // UnMarshal the result Json
        THROW_HR(hr);
    }
    hr = HcnCloseLoadBalancer(handle.get());
    if (FAILED(hr))
    {
        // UnMarshal  the result Json
        THROW_HR(hr);
    }
```
### <a name="enumerate-all-load-balancers"></a>Enumerar todos los equilibradores de carga
En este ejemplo se muestra cómo usar la API de servicio de red de compute de host para enumerar todos los Load Balancer de red de proceso host.
```C++
    wil::unique_cotaskmem_string resultLoadBalancers;
    wil::unique_cotaskmem_string errorRecord;
    std::wstring filter [] = LR"(
    {
         // Future
    })";
    HRESULT result = HcnEnumerateLoadBalancers(filter.c_str(), & resultLoadbalancers, &errorRecord);
    if (FAILED(result))
    {
            // UnMarshal  the result Json
            THROW_HR(result);
    }
```
### <a name="query-load-balancer-properties"></a>Consultar las propiedades del equilibrador de carga
En este ejemplo se muestra cómo usar la API de servicio de red de compute de host para consultar las propiedades de LoadBalancer de la red de proceso del host.
```C++
    unique_hcn_loadbalancer handle;
    GUID lbGuid; // Initialize it to appropriate loadbalancer guid value
    HRESULT hr = HcnOpenLoadBalancer(lbGuid, &handle, &errorRecord);
    if (FAILED(hr))
    {
        // UnMarshal  the result Json
        THROW_HR(hr);
    }
    wil::unique_cotaskmem_string errorRecord;
    wil::unique_cotaskmem_string properties;
    std:wstring query = LR"(
    {
        "ID"  : "",
        "Type" : 0,
    })";
    hr = HcnQueryNProperties(handle.get(), query.c_str(), &properties, &errorRecord);
    if (FAILED(hr))
    {
        // UnMarshal  the result Json
        THROW_HR(hr);
    }
```
## <a name="scenario-hcn-notifications"></a>Escenario: notificaciones de HCN
### <a name="register-and-unregister-service-wide-notifications"></a>Registro y anulación del registro de notificaciones de todo el servicio
En este ejemplo se muestra cómo usar la API de servicio de red de proceso de host para registrar y anular el registro para las notificaciones de todo el servicio. Esto permite que el llamador reciba una notificación (a través de la función de devolución de llamada que especificó durante el registro) cada vez que se ha producido una operación de servicio, como un nuevo evento de creación de red.
```C++
using unique_hcn_callback = wil::unique_any<
    HCN_CALLBACK,
    decltype(&HcnUnregisterServiceCallback),
    HcnUnregisterServiceCallback>;
// Callback handle returned by registration api. Kept at
// global or module scope as it will automatically be
// unregistered when it goes out of scope.
unique_hcn_callback g_Callback;
// Event notification callback function.
void
CALLBACK
ServiceCallback(
    DWORD   NotificationType,
    void*   Context,
    HRESULT NotificationStatus,
    PCWSTR  NotificationData)
{
    // Optional client context
    UNREFERENCED_PARAMETER(context);
    // Reserved for future use
    UNREFERENCED_PARAMETER(NotificationStatus);
    switch (NotificationType)
    {
        case HcnNotificationNetworkCreate:
            // TODO: UnMarshal the NotificationData
            //
            // // Notification
            // {
            //     "ID" : Guid,
            //     "Flags" : <uint32>,
            // };
            break;
        case HcnNotificationNetworkDelete:
            // TODO: UnMarshal the NotificationData
            break;
        Default:
            // TODO: handle other events.
            break;
    }
}
/// Register for service-wide notifications
void RegisterForServiceNotifications()
{
    THROW_IF_FAILED(HcnRegisterServiceCallback(
        ServiceCallback,
        nullptr,
        &g_Callback));
}
/// Unregister from service-wide notifications
void UnregisterForServiceNotifications()
{
    // As this is a unique_hcn_callback, this will cause HcnUnregisterServiceCallback to be invoked
    g_Callback.reset();
}
```
## <a name="next-steps"></a>Pasos siguientes
- Obtenga más información sobre los [identificadores de contexto RPC para HCN](hcn-declaration-handles.md).
- Más información sobre los [esquemas de documento JSON de HCN](hcn-json-document-schemas.md).