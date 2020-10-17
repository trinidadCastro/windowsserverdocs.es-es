---
title: Medición de salida en la red virtual
description: 'Un aspecto fundamental de la monetización de la red en la nube es la salida del ancho de banda de red. Por ejemplo: transferencias de datos salientes en Microsoft Azure modelo de negocio. Los datos de salida se cobran en función de la cantidad total de datos que salen de los centros de datos de Azure a través de Internet en un ciclo de facturación determinado.'
manager: grcusanz
ms.topic: get-started-article
ms.author: anpaul
author: AnirbanPaul
ms.date: 10/02/2018
ms.openlocfilehash: a9e939b4a810848e91b5d2cb8e4b878bbcf56e84
ms.sourcegitcommit: f45640cf4fda621b71593c63517cfdb983d1dc6a
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/17/2020
ms.locfileid: "92156463"
---
# <a name="egress-metering-in-a-virtual-network"></a>Medición de salida en una red virtual

>Se aplica a: Windows Server 2019


Un aspecto fundamental de la monetización de la red en la nube es la posibilidad de facturar según el uso de ancho de banda de red. Los datos de salida se cobran en función de la cantidad total de datos que salen del centro de datos a través de Internet en un ciclo de facturación determinado.

La medición de salida del tráfico de red de SDN en Windows Server 2019 permite ofrecer medidores de uso para las transferencias de datos de salida. El tráfico de red que deja cada red virtual, pero que permanece dentro del centro de datos, puede realizarse por separado, por lo que se puede excluir de los cálculos de facturación. Se realiza un seguimiento de los paquetes enlazados a las direcciones IP de destino que no se incluyen en uno de los intervalos de direcciones sin facturar como transferencias de datos de salida facturadas.

## <a name="virtual-network-unbilled-address-ranges-allowlist-of-ip-ranges"></a>Intervalos de direcciones no facturados de red virtual (permitidos de intervalos IP)

Puede encontrar intervalos de direcciones no facturados en la propiedad **UnbilledAddressRanges** de una red virtual existente. De forma predeterminada, no se han agregado intervalos de direcciones.

   ```PowerShell
   import-module NetworkController
   $uri = "https://sdn.contoso.com"

   (Get-NetworkControllerVirtualNetwork -ConnectionURI $URI -ResourceId "VNet1").properties
   ```

La salida tendrá un aspecto similar al siguiente:
   ```
    AddressSpace           : Microsoft.Windows.NetworkController.AddressSpace
    DhcpOptions            :
    UnbilledAddressRanges  :
    ConfigurationState     :
    ProvisioningState      : Succeeded
    Subnets                : {21e71701-9f59-4ee5-b798-2a9d8c2762f0, 5f4758ef-9f96-40ca-a389-35c414e996cc,
                         29fe67b8-6f7b-486c-973b-8b9b987ec8b3}
    VirtualNetworkPeerings :
    EncryptionCredential   :
    LogicalNetwork         : Microsoft.Windows.NetworkController.LogicalNetwork
   ```


## <a name="example-manage-the-unbilled-address-ranges-of-a-virtual-network"></a>Ejemplo: administración de los intervalos de direcciones sin facturar de una red virtual

Puede administrar el conjunto de prefijos de subred IP para excluir de la medición de salida facturada mediante el establecimiento de la propiedad **UnbilledAddressRange** de una red virtual.  Todo el tráfico enviado por interfaces de red en la red virtual con una dirección IP de destino que coincida con uno de los prefijos no se incluirá en la propiedad BilledEgressBytes.

1.  Actualice la propiedad **UnbilledAddressRanges** para que contenga las subredes a las que no se le facturará el acceso.

    ```PowerShell
    $vnet = Get-NetworkControllerVirtualNetwork -ConnectionUri $uri -ResourceID "VNet1"
    $vnet.Properties.UnbilledAddressRanges = "10.10.2.0/24,10.10.3.0/24"
    ```

    >[!TIP]
    >Si agrega varias subredes IP, use una coma entre cada una de las subredes IP.  No incluya espacios antes ni después de la coma.

2.  Actualice el recurso Virtual Network con la propiedad **UnbilledAddressRanges** modificada.

    ```PowerShell
    New-NetworkControllerVirtualNetwork -ConnectionUri $uri -ResourceId "VNet1" -Properties $unbilled.Properties -PassInnerException
    ```

    La salida tendrá un aspecto similar al siguiente:
      ```
         Confirm
         Performing the operation 'New-NetworkControllerVirtualNetwork' on entities of type
         'Microsoft.Windows.NetworkController.VirtualNetwork' via
         'https://sdn.contoso.com/networking/v3/virtualNetworks/VNet1'. Are you sure you want to continue?
         [Y] Yes  [N] No  [S] Suspend  [?] Help (default is "Y"): y


         Tags             :
         ResourceRef      : /virtualNetworks/VNet1
         InstanceId       : 29654b0b-9091-4bed-ab01-e172225dc02d
         Etag             : W/"6970d0a3-3444-41d7-bbe4-36327968d853"
         ResourceMetadata :
         ResourceId       : VNet1
         Properties       : Microsoft.Windows.NetworkController.VirtualNetworkProperties
      ```


3. Compruebe la Virtual Network para ver el **UnbilledAddressRanges**configurado.

   ```PowerShell
   (Get-NetworkControllerVirtualNetwork -ConnectionUri $uri -ResourceID "VNet1").properties
   ```

   La salida tendrá un aspecto similar al siguiente:
   ```
   AddressSpace           : Microsoft.Windows.NetworkController.AddressSpace
   DhcpOptions            :
   UnbilledAddressRanges  : 10.10.2.0/24,192.168.2.0/24
   ConfigurationState     :
   ProvisioningState      : Succeeded
   Subnets                : {21e71701-9f59-4ee5-b798-2a9d8c2762f0, 5f4758ef-9f96-40ca-a389-35c414e996cc,
                        29fe67b8-6f7b-486c-973b-8b9b987ec8b3}
   VirtualNetworkPeerings :
   EncryptionCredential   :
   LogicalNetwork         : Microsoft.Windows.NetworkController.LogicalNetwork
   ```

## <a name="check-the-billed-the-unbilled-egress-usage-of-a-virtual-network"></a>Comprobación del uso facturado de la salida sin facturar de una red virtual

Después de configurar la propiedad **UnbilledAddressRanges** , puede comprobar el uso de salida facturada y sin facturar de cada subred de una red virtual. El tráfico de salida se actualiza cada cuatro minutos con el total de bytes de los intervalos facturados y sin facturar.

Las siguientes propiedades están disponibles para cada subred virtual:

-   **UnbilledEgressBytes** muestra el número de bytes no facturados enviados por interfaces de red conectadas a esta subred virtual. Los bytes no facturados son bytes enviados a intervalos de direcciones que forman parte de la propiedad **UnbilledAddressRanges** de la red virtual principal.

-   **BilledEgressBytes** muestra el número de bytes facturados enviados por interfaces de red conectadas a esta subred virtual. Los bytes facturados son bytes enviados a intervalos de direcciones que no forman parte de la propiedad **UnbilledAddressRanges** de la red virtual principal.

Use el ejemplo siguiente para consultar el uso de salida:

```PowerShell
(Get-NetworkControllerVirtualNetwork -ConnectionURI $URI -ResourceId "VNet1").properties.subnets.properties | ft AddressPrefix,BilledEgressBytes,UnbilledEgressBytes
```

La salida tendrá un aspecto similar al siguiente:
```
AddressPrefix BilledEgressBytes UnbilledEgressBytes
------------- ----------------- -------------------
10.0.255.8/29          16827067                   0
10.0.2.0/24           781733019                   0
10.0.4.0/24                   0                   0
```


---
