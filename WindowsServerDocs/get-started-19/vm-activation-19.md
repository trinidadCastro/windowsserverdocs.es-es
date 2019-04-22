---
title: Activación automática de máquina virtual
TOCTitle: Automatic VM Activation
description: Cómo activar las máquinas virtuales en Windows Server 2012 R2, Windows Server 2016 y Windows Server 2019
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
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59822096"
---
# <a name="automatic-virtual-machine-activation"></a>Activación automática de máquina virtual

> Se aplica a: Windows Server 2019, canal semianual de Windows Server, Windows Server 2016, Windows Server 2012 R2

La activación automática de máquina virtual (AVMA) actúa como un mecanismo de prueba de compra y ayuda a garantizar que los productos Windows se utilicen en conformidad con los Derechos de uso del producto y los Términos de licencia del software de Microsoft.

AVMA permite instalar máquinas virtuales en un servidor Windows que esté correctamente activado sin tener que administrar las claves de producto de cada máquina virtual individual, incluso en entornos desconectados. AVMA enlaza la activación de la máquina virtual con el servidor de virtualización con licencia y activa la máquina virtual cuando se inicia. AVMA también ofrece informes en tiempo real sobre el uso y datos históricos sobre el estado de licencia de la máquina virtual. Los datos de informes y seguimiento están disponibles en el servidor de virtualización.

## <a name="practical-applications"></a>Aplicaciones prácticas

En los servidores de virtualización que están activados con licencias por volumen o con licencias OEM, AVMA ofrece varias ventajas.

Los administradores de centros de datos de servidores pueden usar AVMA para hacer lo siguiente:

  - Activar máquinas virtuales en ubicaciones remotas

  - Activar máquinas virtuales con o sin conexión a Internet

  - Hacer un seguimiento del uso y las licencias de las máquinas virtuales desde el servidor de virtualización, sin requerir derechos de acceso en los sistemas virtualizados

No hay que administrar claves de producto ni leer adhesivos en los servidores. La máquina virtual se activa y sigue funcionando incluso cuando se migra entre una matriz de servidores de virtualización.

Los partners de Contrato de licencia de proveedor de servicios (SPLA) y otros proveedores de hospedaje no tienen que compartir claves de producto con inquilinos ni tener acceso a la máquina virtual de un inquilino para activarla. La activación de máquinas virtuales es transparente para el inquilino cuando se usa AVMA. Los proveedores de hospedaje pueden usar los registros del servidor para comprobar el cumplimiento de las licencias y hacer un seguimiento del historial de uso de clientes.

## <a name="system-requirements"></a>Requisitos del sistema

AVMA requiere un servidor de virtualización de Microsoft que ejecutan Windows Server 2012 R2, Windows Server 2016 Datacenter o Windows Server Datacenter de 2019. 

Estos son los invitados que pueden activar los hosts de versión diferentes:

|Versión del servidor host|Windows Server 2019|Windows Server 2016|Windows Server 2012 R2|
|-|-|-|-|
|Windows Server 2019|X|X|X|
|Windows Server 2016| |X|X|
|Windows Server 2012 R2| ||X|

Tenga en cuenta que estos activación todas las ediciones (Essentials, Standard o Datacenter).

Esta herramienta no funciona con otras tecnologías de servidor de virtualización.

## <a name="how-to-implement-avma"></a>Cómo implementar AVMA

1.  En un servidor de virtualización de Windows Server Datacenter, instale y configure el rol de servidor de Hyper-V de Microsoft. Para obtener más información, consulte [instalar Hyper-V Server](../virtualization/hyper-v/get-started/install-the-hyper-v-role-on-windows-server.md).

2.  [Crear una máquina virtual](../virtualization/hyper-v/get-started/create-a-virtual-machine-in-hyper-v.md) e instalar un sistema operativo de servidor admitidos en ella.

3.  Instale la clave de AVMA en la máquina virtual. Desde un símbolo del sistema con privilegios elevados, ejecute el siguiente comando:
    
    ``` 
    slmgr /ipk <AVMA_key>  
    ```

La máquina virtual activará automáticamente la licencia frente al servidor de virtualización.


> [!TIP]
> También puede emplear las claves de AVMA en cualquier archivo de configuración de tipo Unattend.exe.


## <a name="avma-keys"></a>Claves de AVMA

Las siguientes claves AVMA pueden utilizarse para Windows Server 2019.

|Edición|   Clave de AVMA|
|-|-|
|Datacenter|    H3RNG-8C32Q-Q8FRX-6TDXV-WMBMW|
|Standard|  TNK62-RXVTB-4P47B-2D623-4GF74|
|Essentials|    2CTP7-NHT64-BP62M-FV6GG-HFV28|
 
Las siguientes claves AVMA pueden utilizarse para Windows Server, versión 1809.

|Edición|   Clave de AVMA|
|-|-|
|Datacenter|    H3RNG-8C32Q-Q8FRX-6TDXV-WMBMW|
|Standard|  TNK62-RXVTB-4P47B-2D623-4GF74|

Las siguientes claves AVMA pueden utilizarse para Windows Server, versión 1803 y 1709.

|Edición|Clave de AVMA|
|-|-|
|Datacenter|TMJ3Y-NTRTM-FJYXT-T22BY-CWG3J|
|Standard|C3RCX-M6NRP-6CXC9-TW2F2-4RHYD|


Las siguientes claves AVMA pueden utilizarse para Windows Server 2016.

|Edición|Clave de AVMA|
|-|-|
|Datacenter|TMJ3Y-NTRTM-FJYXT-T22BY-CWG3J|
|Standard|C3RCX-M6NRP-6CXC9-TW2F2-4RHYD|
|Essentials|B4YNW-62DX9-W8V6M-82649-MHBKQ|


Pueden utilizarse las siguientes claves de AVMA para Windows Server 2012 R2.

|Edición|Clave de AVMA|
|-|-|
|Datacenter|Y4TGP-NPTV9-HTC2H-7MGQ3-DV4TW|
|Standard|DBGBW-NPF86-BJVTX-K3WKJ-MTB6V|
|Essentials|K2XGM-NMBT3-2R6Q8-WF2FK-P36R2|

## <a name="reporting-and-tracking"></a>Informes y seguimiento

El Registro (KVP) en el servidor de virtualización proporciona datos de seguimiento en tiempo real para los sistemas operativos invitados. Dado que la clave del Registro se mueve con la máquina virtual, puede obtener también información de la licencia. De forma predeterminada, KVP devuelve información acerca de la máquina virtual, incluyendo lo siguiente:

  - Nombre de dominio completo

  - Sistema operativo y paquetes de servicio instalados

  - Arquitectura de procesador

  - Direcciones de red IPv4 e IPv6

  - Direcciones RDP

Para obtener más información acerca de cómo obtener esta información, consulte [Hyper-V Script: Observación de GuestIntrinsicExchangeItems KVP en](http://blogs.msdn.com/b/virtual_pc_guy/archive/2008/11/18/hyper-v-script-looking-at-kvp-guestintrinsicexchangeitems.aspx).


> [!NOTE]
> Los datos de KVP no están protegidos. Pueden modificarse y no se supervisan ante posibles cambios.



> [!IMPORTANT]
> Los datos de KVP deben quitarse si la clave de AVMA se sustituye por otra clave de producto (clave OEM, de licencia por volumen o comercial).


Los datos históricos acerca de las solicitudes de AVMA están disponibles en un archivo de registro en el servidor de virtualización (id. de evento 12310).

Dado que el proceso de activación de AVMA es transparente, los mensajes de error no se muestran. No obstante, los eventos siguientes se capturan en un archivo de registro en las máquinas virtuales (id. de evento 12309).

|Notification|Descripción|
|-|-|
|Éxito de AVMA|El equipo virtual se ha activado.|
|Host no válido|El servidor de virtualización deja de responder. Esto puede suceder cuando el servidor no ejecuta una versión compatible de Windows.|
|Datos no válidos|Esto normalmente es el resultado de un error de comunicación entre el servidor de virtualización y la máquina virtual, a menudo provocado por datos no coincidentes, dañados o cifrados.|
|Activación denegada|El servidor de virtualización no ha podido activar el sistema operativo invitado porque el identificador de AVMA no coincide.|

