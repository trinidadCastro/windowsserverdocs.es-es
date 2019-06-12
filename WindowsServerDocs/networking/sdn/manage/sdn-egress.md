---
title: Medición de salida en la red virtual
description: Un aspecto fundamental de la monetización de red en la nube es la salida de ancho de banda de red. Por ejemplo, el modelo de negocio en Microsoft Azure de transferencias de datos salientes. Datos de salida se cobran según la cantidad total de datos que se sale de los centros de datos de Azure a través de Internet durante un ciclo de facturación determinado.
manager: dougkim
ms.prod: windows-server-threshold
ms.technology: networking-hv-switch
ms.topic: get-started-article
ms.assetid: ''
ms.author: pashort
author: shortpatti
ms.date: 10/02/2018
ms.openlocfilehash: 50aee16b0b5797f28ebcdf61494b09669699873f
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/31/2019
ms.locfileid: "66446322"
---
# <a name="egress-metering-in-a-virtual-network"></a>Medición de salida en una red virtual

>Se aplica a: Windows Server 2019


Un aspecto fundamental de la monetización de red en la nube es poder facturar el uso de ancho de banda de red. Datos de salida se cobran según la cantidad total de datos que se sale del centro de datos a través de Internet durante un ciclo de facturación determinado.

Medición de salida para tráfico de red SDN en Windows Server 2019 permite la capacidad para ofrecer los medidores de uso para las transferencias de datos de salida. Tráfico de red que sale de cada red virtual, pero permanece dentro del centro de datos puede realiza el seguimiento por separado para que se puede excluir de cálculos de facturación. Se realiza un seguimiento de los paquetes destinados a direcciones IP de destino que no están incluidas en uno de los intervalos de direcciones no facturados como facturarán las transferencias de datos de salida.

## <a name="virtual-network-unbilled-address-ranges-whitelist-of-ip-ranges"></a>(Lista blanca de intervalos de direcciones IP) de intervalos de direcciones de red virtual no facturado

Puede encontrar los intervalos de direcciones no facturado en el **UnbilledAddressRanges** propiedad de una red virtual existente. De forma predeterminada, no hay ningún intervalo de direcciones agregado.

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


## <a name="example-manage-the-unbilled-address-ranges-of-a-virtual-network"></a>Por ejemplo: Administrar los intervalos de direcciones no facturado de una red virtual

Puede administrar el conjunto de prefijos de subred IP para excluir de la medición de salida facturado estableciendo el **UnbilledAddressRange** propiedad de una red virtual.  No se incluirá en la propiedad BilledEgressBytes todo el tráfico enviado en la red virtual con una dirección IP de destino que coincide con uno de los prefijos de las interfaces de red.

1.  Actualización de la **UnbilledAddressRanges** propiedad para que contenga las subredes que no se facturará para el acceso.

    ```PowerShell
    $vnet = Get-NetworkControllerVirtualNetwork -ConnectionUri $uri -ResourceID "VNet1"
    $vnet.Properties.UnbilledAddressRanges = "10.10.2.0/24,10.10.3.0/24"
    ```

    >[!TIP]
    >Si agrega varias subredes IP, use una coma entre cada una de las subredes IP.  No incluya espacios en blanco antes o después de la coma.

2.  Actualizar el recurso de red Virtual con la modificación **UnbilledAddressRanges** propiedad.

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


~~~
Tags             :
ResourceRef      : /virtualNetworks/VNet1
InstanceId       : 29654b0b-9091-4bed-ab01-e172225dc02d
Etag             : W/"6970d0a3-3444-41d7-bbe4-36327968d853"
ResourceMetadata :
ResourceId       : VNet1
Properties       : Microsoft.Windows.NetworkController.VirtualNetworkProperties
```
~~~


3. Check the Virtual Network to see the configured **UnbilledAddressRanges**.

   ```PowerShell
   (Get-NetworkControllerVirtualNetwork -ConnectionUri $uri -ResourceID "VNet1").properties
   ```

   Your output will now look similar to this:
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

## Check the billed the unbilled egress usage of a virtual network

After you configure the **UnbilledAddressRanges** property, you can check the billed and unbilled egress usage of each subnet within a virtual network. Egress traffic updates every four minutes with the total bytes of the billed and unbilled ranges.

The following properties are available for each virtual subnet:

-   **UnbilledEgressBytes** shows the number of unbilled bytes sent by network interfaces connected to this virtual subnet. Unbilled bytes are bytes sent to address ranges that are part of the **UnbilledAddressRanges** property of the parent virtual network.

-   **BilledEgressBytes** shows Number of billed bytes sent by network interfaces connected to this virtual subnet. Billed bytes are bytes sent to address ranges that are not part of the **UnbilledAddressRanges** property of the parent virtual network.

Use the following example to query egress usage:

```PowerShell
(Get-NetworkControllerVirtualNetwork -ConnectionURI $URI -ResourceId "VNet1").properties.subnets.properties | ft AddressPrefix,BilledEgressBytes,UnbilledEgressBytes
```

Your output will look similar to this:
```
AddressPrefix BilledEgressBytes UnbilledEgressBytes
------------- ----------------- -------------------
10.0.255.8/29          16827067                   0
10.0.2.0/24           781733019                   0
10.0.4.0/24                   0                   0
```


---