---
title: Activación automática de máquina virtual
TOCTitle: Automatic VM Activation
description: Activación de máquinas virtuales en Windows Server 2019, Windows Server 2016 y Windows Server 2012 R2
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: server-general
ms.tgt_pltfrm: na
ms.topic: article
author: lizap
ms.author: elizapo
ms.localizationpriority: medium
ms.openlocfilehash: 43c0ce500058bd4115d58b68dc79068a52c0bb3e
ms.sourcegitcommit: b9ec35416a06854c1bc875a2b731d42a436fe313
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/12/2019
ms.locfileid: "73956094"
---
# <a name="automatic-virtual-machine-activation"></a>Activación automática de máquina virtual

> Se aplica a: Windows Server 2019, Windows Server (Canal semianual), Windows Server 2016, Windows Server 2012 R2

La activación automática de máquina virtual (AVMA) actúa como un mecanismo de prueba de compra y ayuda a garantizar que los productos Windows se utilicen en conformidad con los Derechos de uso del producto y los Términos de licencia del software de Microsoft.

AVMA permite instalar máquinas virtuales en un servidor Windows que esté correctamente activado sin tener que administrar las claves de producto de cada máquina virtual individual, incluso en entornos desconectados. AVMA enlaza la activación de la máquina virtual con el servidor de virtualización con licencia y activa la máquina virtual cuando se inicia. AVMA también ofrece informes en tiempo real sobre el uso y datos históricos sobre el estado de licencia de la máquina virtual. Los datos de informes y seguimiento están disponibles en el servidor de virtualización.

## <a name="practical-applications"></a>Aplicaciones prácticas

En los servidores de virtualización que están activados con licencias por volumen o con licencias OEM, AVMA ofrece varias ventajas.

Los administradores de centros de datos de servidores pueden usar AVMA para hacer lo siguiente:

  - Activar máquinas virtuales en ubicaciones remotas

  - Activar máquinas virtuales con o sin conexión a Internet

  - Hacer un seguimiento del uso y las licencias de las máquinas virtuales desde el servidor de virtualización, sin requerir derechos de acceso en los sistemas virtualizados

No hay que administrar claves de producto ni leer adhesivos en los servidores. La máquina virtual se activa y sigue funcionando incluso cuando se migra entre una matriz de servidores de virtualización.

Los asociados de Contrato de licencia de proveedor de servicios (SPLA) y otros proveedores de hospedaje no tienen que compartir claves de producto con inquilinos ni tener acceso a la máquina virtual de un inquilino para activarla. La activación de máquinas virtuales es transparente para el inquilino cuando se usa AVMA. Los proveedores de hospedaje pueden usar los registros del servidor para comprobar el cumplimiento de las licencias y hacer un seguimiento del historial de uso de clientes.

## <a name="system-requirements"></a>Requisitos del sistema

AVMA requiere una instancia de Microsoft Virtualization Server que ejecute Windows Server 2019, Windows Server 2016 Datacenter o Windows Server 2012 R2. 

Estos son los invitados que los hosts de diferentes versiones pueden activar:

|Versión del servidor host|Windows Server 2019|Windows Server 2016|Windows Server 2012 R2|
|-|-|-|-|
|Windows Server 2019|X|X|X|
|Windows Server 2016| |X|X|
|Windows Server 2012 R2| ||X|

Observa que estas activan todas las ediciones (Datacenter, Standard o Essentials).

Esta herramienta no funciona con otras tecnologías de servidor de virtualización.

## <a name="how-to-implement-avma"></a>Cómo implementar AVMA

1.  En un servidor de virtualización Windows Server Datacenter, instala y configura el rol de Microsoft Hyper-V Server. Para más información, consulta el tema sobre la [instalación de Hyper-V Server](../virtualization/hyper-v/get-started/install-the-hyper-v-role-on-windows-server.md).

2.  [Crea una máquina virtual](../virtualization/hyper-v/get-started/create-a-virtual-machine-in-hyper-v.md) e instala en ella un sistema operativo de servidor compatible.

3.  Instale la clave de AVMA en la máquina virtual. Desde un símbolo del sistema con privilegios elevados, ejecute el siguiente comando:
    
    ``` 
    slmgr /ipk <AVMA_key>  
    ```

La máquina virtual activará automáticamente la licencia frente al servidor de virtualización.


> [!TIP]
> También puede emplear las claves de AVMA en cualquier archivo de configuración de tipo Unattend.exe.


## <a name="avma-keys"></a>Claves de AVMA

Pueden utilizarse las siguientes claves de AVMA para Windows Server 2019.

|Edición|   Clave de AVMA|
|-|-|
|Datacenter|    H3RNG-8C32Q-Q8FRX-6TDXV-WMBMW|
|Standard|  TNK62-RXVTB-4P47B-2D623-4GF74|
|Essentials|    2CTP7-NHT64-BP62M-FV6GG-HFV28|
 
Pueden usarse las siguientes claves de AVMA para Windows Server, versiones 1909, 1903 y 1809.

|Edición|   Clave de AVMA|
|-|-|
|Datacenter|    H3RNG-8C32Q-Q8FRX-6TDXV-WMBMW|
|Standard|  TNK62-RXVTB-4P47B-2D623-4GF74|

Pueden utilizarse las siguientes claves de AVMA para Windows Server, versiones 1803 y 1709.

|Edición|Clave de AVMA|
|-|-|
|Datacenter|TMJ3Y-NTRTM-FJYXT-T22BY-CWG3J|
|Standard|C3RCX-M6NRP-6CXC9-TW2F2-4RHYD|


Pueden utilizarse las siguientes claves de AVMA para Windows Server 2016.

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

Para más información sobre cómo obtener esta información, consulta el tema acerca del [script de Hyper-V: observación de GuestIntrinsicExchangeItems en KVP](https://blogs.msdn.com/b/virtual_pc_guy/archive/2008/11/18/hyper-v-script-looking-at-kvp-guestintrinsicexchangeitems.aspx).


> [!NOTE]
> Los datos de KVP no están protegidos. Pueden modificarse y no se supervisan ante posibles cambios.



> [!IMPORTANT]
> Los datos de KVP deben quitarse si la clave de AVMA se sustituye por otra clave de producto (clave OEM, de licencia por volumen o comercial).


Los datos históricos acerca de las solicitudes de AVMA están disponibles en un archivo de registro en el servidor de virtualización (id. de evento 12310).

Dado que el proceso de activación de AVMA es transparente, los mensajes de error no se muestran. No obstante, los eventos siguientes se capturan en un archivo de registro en las máquinas virtuales (id. de evento 12309).

|Notificación|Descripción|
|-|-|
|Éxito de AVMA|El equipo virtual se ha activado.|
|Host no válido|El servidor de virtualización deja de responder. Esto puede suceder cuando el servidor no ejecuta una versión compatible de Windows.|
|Datos no válidos|Esto normalmente es el resultado de un error de comunicación entre el servidor de virtualización y la máquina virtual, a menudo provocado por datos no coincidentes, dañados o cifrados.|
|Activación denegada|El servidor de virtualización no ha podido activar el sistema operativo invitado porque el identificador de AVMA no coincide.|

