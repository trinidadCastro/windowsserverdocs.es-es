---
title: Activación de la máquina virtual automática
TOCTitle: Automatic VM Activation
description: Para activar las máquinas virtuales en Windows Server 2019, Windows Server 2016 y Windows Server 2012 R2
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: server-general
ms.tgt_pltfrm: na
ms.topic: article
author: lizap
ms.author: elizapo
ms.localizationpriority: medium
ms.openlocfilehash: 62873140c8e114ba537dc4fd3ff7c44868c33243
ms.sourcegitcommit: ca5c80bdb903b282e292488172a7cc92546574c0
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/04/2018
ms.locfileid: "4375492"
---
# Activación de la máquina virtual automática

> Se aplica a: Windows Server 2019, canal semianual de Windows Server, Windows Server 2016, Windows Server 2012 R2

Activación de máquina Virtual automática (AVMA) actúa como un mecanismo de prueba de compra, lo que ayuda a garantizar que los productos de Windows se usan con arreglo a los derechos de uso del producto y los términos de licencia del Software de Microsoft.

AVMA te permite instalar las máquinas virtuales en un servidor de Windows activado correctamente sin tener que administrar claves de producto para cada máquina virtual individual, incluso en entornos desconectados. AVMA enlaza la activación de la máquina virtual con el servidor de virtualización con licencia y activa la máquina virtual cuando se inicia. AVMA también proporciona informes en tiempo real en uso y los datos históricos sobre el estado de licencia de la máquina virtual. Informes y los datos de seguimiento está disponible en el servidor de virtualización.

## Aplicaciones prácticas

En los servidores de virtualización que se activan mediante licencias por volumen o OEM licencias, AVMA ofrece varias ventajas.

Los administradores de centros de datos de servidor pueden utilizar AVMA para hacer lo siguiente:

  - Activar máquinas virtuales en ubicaciones remotas

  - Activar las máquinas virtuales con o sin una conexión a internet

  - Realizar un seguimiento de uso de la máquina virtual y las licencias desde el servidor de virtualización, sin necesidad de los derechos de acceso en los sistemas virtualizados

Hay sin claves de producto para administrar y no hay etiquetas en los servidores de leer. La máquina virtual se activa y continúa trabajando incluso cuando se migra a través de una matriz de servidores de virtualización.

Los partners de contrato de licencia de proveedor de servicio (SPLA) y otros proveedores de hospedaje no tienen que compartir las claves de producto con inquilinos o acceder a la máquina virtual del inquilino para activarlo. Activación de la máquina virtual es transparente para el inquilino cuando se usa AVMA. Alojamiento de proveedores de usar los registros del servidor para comprobar el cumplimiento de licencia y realizar un seguimiento de historial de uso de cliente.

## Requisitos del sistema

AVMA requiere un servidor de virtualización de Microsoft que ejecutan Windows Server 2019 Datacenter, Windows Server 2016 Datacenter o Windows Server 2012 R2. 

Estos son los invitados que se pueden activar los hosts de versión diferente:

|Versión de host del servidor|Windows Server 2019|WindowsServer2016|Windows Server2012R2|
|-|-|-|-|
|Windows Server 2019|X|X|X|
|WindowsServer2016| |X|X|
|Windows Server2012R2| ||X|

Ten en cuenta que estos activen todas las ediciones (Datacenter, Standard o Essentials).

Esta herramienta no funciona con otras tecnologías de servidor de virtualización.

## Cómo implementar AVMA

1.  En un servidor de virtualización de Windows Server Datacenter, instalar y configurar el rol de servidor de Hyper-V de Microsoft. Para obtener más información, consulta [Instalar Hyper-V Server](../virtualization/hyper-v/get-started/install-the-hyper-v-role-on-windows-server.md).

2.  [Crear una máquina virtual](../virtualization/hyper-v/get-started/create-a-virtual-machine-in-hyper-v.md) e instalar un sistema operativo de servidor compatible en él.

3.  Instala la clave AVMA en la máquina virtual. Desde un símbolo del sistema con privilegios elevados, ejecuta el siguiente comando:
    
    ``` 
    slmgr /ipk <AVMA_key>  
    ```

La máquina virtual se activará automáticamente la licencia en el servidor de virtualización.


> [!TIP]
> También puede emplear las claves AVMA en cualquier archivo de instalación Unattend.exe.


## Claves AVMA

Las siguientes claves AVMA pueden usarse para Windows Server 2019.

|Edición|   Clave AVMA|
|-|-|
|Datacenter|    H3RNG 8C32Q Q8FRX 6TDXV WMBMW|
|Standard|  TNK62-RXVTB-4P47B - 2D 623-4GF74|
|Conceptos básicos|    2CTP7 NHT64 BP62M FV6GG HFV28|
 
Las siguientes claves AVMA pueden usarse para Windows Server, versión 1809.

|Edición|   Clave AVMA|
|-|-|
|Datacenter|    H3RNG 8C32Q Q8FRX 6TDXV WMBMW|
|Standard|  TNK62-RXVTB-4P47B - 2D 623-4GF74|

Las siguientes claves AVMA pueden usarse para Windows Server, versión 1803 y 1709.

|Edición|Clave AVMA|
|-|-|
|Datacenter|TMJ3Y NTRTM FJYXT T22BY CWG3J|
|Standard|C3RCX M6NRP 6CXC9 TW2F2 4RHYD|


Las siguientes claves AVMA pueden usarse para Windows Server 2016.

|Edición|Clave AVMA|
|-|-|
|Datacenter|TMJ3Y NTRTM FJYXT T22BY CWG3J|
|Standard|C3RCX M6NRP 6CXC9 TW2F2 4RHYD|
|Conceptos básicos|B4YNW 62DX9 W8V6M 82649 MHBKQ|


Las siguientes claves AVMA pueden usarse para Windows Server 2012 R2.

|Edición|Clave AVMA|
|-|-|
|Datacenter|Y4TGP NPTV9 HTC2H 7MGQ3 DV4TW|
|Standard|DBGBW NPF86 BJVTX K3WKJ MTB6V|
|Conceptos básicos|K2XGM NMBT3 2R6Q8 WF2FK P36R2|

## Informes y seguimiento

El registro (KVP) en el servidor de virtualización proporciona datos de seguimiento en tiempo real para los sistemas operativos invitados. Dado que la clave del registro se mueve con la máquina virtual, puedes obtener, así como información de licencia. De manera predeterminada la KVP devuelve información sobre la máquina virtual, incluyendo lo siguiente:

  - Nombre de dominio completo

  - Sistema operativo y service packs instalados

  - Arquitectura de procesador

  - Direcciones de red IPv4 e IPv6

  - Direcciones RDP

Para obtener más información sobre cómo obtener esta información, consulta [Script de Hyper-V: mirando a KVP GuestIntrinsicExchangeItems](http://blogs.msdn.com/b/virtual_pc_guy/archive/2008/11/18/hyper-v-script-looking-at-kvp-guestintrinsicexchangeitems.aspx).


> [!NOTE]
> No se protegen los datos KVP. Se puede modificar y no se supervisa los cambios.



> [!IMPORTANT]
> Datos KVP deben quitarse si la clave AVMA se reemplaza con otra clave de producto (clave licencia por volumen, OEM o comercial).


Datos históricos acerca de las solicitudes AVMA están disponibles en un archivo de registro en el servidor de virtualización (EventID 12310).

Dado que el proceso de activación AVMA es transparente, no se muestran los mensajes de error. Sin embargo, los siguientes eventos se capturan en un archivo de registro en las máquinas virtuales (EventID 12309).

|Notificación|Descripción|
|-|-|
|AVMA éxito|La máquina virtual se activó.|
|Host no válido|El servidor de virtualización es que no responden. Esto puede suceder cuando el servidor no se está ejecutando una versión compatible de Windows.|
|Datos no válidos|Esto suele ser debido a un error en la comunicación entre el servidor de virtualización y la máquina virtual, a menudo provocados por error de coincidencia de datos, cifrado o daños.|
|Activación denegada|El servidor de virtualización no pudo activar el sistema operativo invitado porque no coincide con el identificador de AVMA.|

