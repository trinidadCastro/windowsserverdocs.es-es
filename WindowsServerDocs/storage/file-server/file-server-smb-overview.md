---
title: Información general sobre el uso compartido de archivos mediante el protocolo SMB 3 en Windows Server
description: Información general sobre el uso del protocolo SMB 3 para recursos compartidos de archivos y archivos con Windows Server.
ms.prod: windows-server
ms.topic: article
author: JasonGerend
ms.author: jgerend
ms.technology: storage
ms.date: 07/09/2018
ms.localizationpriority: medium
ms.openlocfilehash: b40c179d242a0c48c6eb176db1225979f9e6a123
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71402086"
---
# <a name="overview-of-file-sharing-using-the-smb-3-protocol-in-windows-server"></a>Información general sobre el uso compartido de archivos mediante el protocolo SMB 3 en Windows Server

>Se aplica a: Windows Server 2012 R2, Windows Server 2012 y Windows Server 2016

En este tema se describe la característica SMB 3,0 en Windows Server® 2012, Windows Server 2012 R2 y Windows Server 2016: usos prácticos para la característica, la funcionalidad nueva o actualizada más importante de esta versión en comparación con las versiones anteriores, y el hardware satisfacer.

## <a name="feature-description"></a>Descripción de la característica

El protocolo del Bloque de mensajes del servidor (SMB) es un protocolo de uso compartido de archivos de red que permite que las aplicaciones de un equipo puedan leer y escribir archivos y solicitar servicios desde los programas de un servidor en una red de equipos. El protocolo SMB puede usarse sobre el protocolo TCP/IP u otros protocolos de red. Con el uso de un protocolo SMB, una aplicación (o el usuario de una aplicación) puede acceder a los archivos u otros recursos de un servidor remoto. Esto permite que las aplicaciones puedan leer, crear y actualizar archivos en un servidor remoto. También puede comunicarse con cualquier programa del servidor que esté configurado para recibir la solicitud de un cliente SMB. Windows Server 2012 incluye la nueva versión 3,0 del protocolo SMB.

## <a name="practical-applications"></a>Aplicaciones prácticas

En esta sección se tratan algunas nuevas maneras prácticas para usar el nuevo protocolo SMB 3.0.

* **Almacenamiento de archivos para visualización (Hyper-V™ a través de SMB)** . En Hyper-V se pueden almacenar archivos de máquinas virtuales, como los archivos de configuración, disco duro virtual (VHD) e instantáneas, en recursos de archivos compartidos a través del protocolo SMB 3.0. Esto puede usarse tanto para los servidores de archivos independientes como para los servidores de archivos agrupados que usan Hyper-V con el almacenamiento de archivos compartidos del clúster.
* **Microsoft SQL Server a través de SMB**. En SQL Server se pueden almacenar archivos de base de datos de usuarios en recursos compartidos de archivos SMB. Actualmente, esto se admite con SQL Server 2008 R2 para los servidores SQL independientes. Las próximas versiones de SQL Server admitirán servidores SQL Server en clúster y bases de datos del sistema.
* **Almacenamiento tradicional para datos de usuario final**. El protocolo SMB 3.0 cuenta con mejoras para las cargas de trabajo del trabajador de la información (o cliente). Estas mejoras incluyen la reducción de las latencias de la aplicación que experimentan los usuarios de las sucursales cuando acceden a los datos a través de las redes de área extensa (WAN) y protegen los datos contra los ataques de interceptación.

## <a name="new-and-changed-functionality"></a>Funcionalidad nueva y modificada

Para obtener información sobre la funcionalidad nueva y modificada en Windows Server 2012 R2, consulte [novedades de SMB en Windows Server](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/hh831474(v%3dws.11)>).

SMB en Windows Server 2012 y Windows Server 2016 incluye el nuevo protocolo SMB 3,0 y muchas mejoras nuevas que se describen en la tabla siguiente.

<table>
<colgroup>
<col style="width: 33%" />
<col style="width: 33%" />
<col style="width: 33%" />
</colgroup>
<thead>
<tr class="header">
<th><p>Característica/funcionalidad</p></th>
<th><p>Nueva o actualizada</p></th>
<th><p>Resumen</p></th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><p>Conmutación por error transparente SMB</p></td>
<td><p>Nuevo</p></td>
<td><p>Permite que los administradores puedan realizar mantenimientos de hardware o software de los nodos en un servidor de archivos en clúster sin interrumpir las aplicaciones del servidor que almacenan datos en estos recursos compartidos de archivos. Además, si ocurre un error de hardware o software en el nodo de clúster, los clientes SMB se vuelven a conectar de forma transparente en otro nodo de clúster sin interrumpir las aplicaciones de servidor que están almacenando datos en estos recursos compartidos de archivos.</p></td>
</tr>
<tr class="even">
<td><p>Escalabilidad horizontal SMB</p></td>
<td><p>Nuevo</p></td>
<td><p>Al usar volúmenes compartidos de clúster (CSV) versión 2, los administradores pueden crear recursos compartidos de archivos que proporcionan acceso simultáneo a los archivos de datos, con E/S directa, a través de todos los nodos de un clúster de servidores de archivos. Esto proporciona una mejor utilización del ancho de banda de red y el equilibrio de carga de los clientes de servidores de archivos y optimiza el rendimiento de las aplicaciones de los servidores.</p></td>
</tr>
<tr class="odd">
<td><p>SMB multicanal</p></td>
<td><p>Nuevo</p></td>
<td><p>Permite agregar el ancho de banda de red y la tolerancia a errores de la red cuando hay varias rutas disponibles entre el cliente SMB 3.0 y el servidor SMB 3.0. Esto permite que las aplicaciones de servidores aprovechen todo el ancho de banda de red disponible y sean resistentes a los errores de red.</p></td>
</tr>
<tr class="even">
<td><p>SMB directo</p></td>
<td><p>Nuevo</p></td>
<td><p>Admite el uso de adaptadores de red que tienen capacidades RDMA y pueden funcionar a toda velocidad con una latencia muy baja y, al mismo tiempo, minimizan el uso de CPU. Para las cargas de trabajo como Hyper-V o Microsoft SQL Server, esto permite que un servidor de archivo remoto se parezca al almacenamiento local.</p></td>
</tr>
<tr class="odd">
<td><p>Contadores de rendimiento para las aplicaciones de servidores</p></td>
<td><p>Nuevo</p></td>
<td><p>Los nuevos contadores de rendimiento SMB proporcionan información detallada, por recurso compartido sobre el rendimiento, la latencia y la E/S por segundo (IOPS), lo que les permite a los administradores analizar el rendimiento de los recursos compartidos del rendimiento SMB 3.0 donde están almacenados sus datos. Estos contadores están especialmente diseñados para las aplicaciones de servidores, como Hyper-V y SQL Server, que almacenan archivos en los recursos compartidos de archivos remotos.</p></td>
</tr>
<tr class="even">
<td><p>Optimizaciones de rendimiento</p></td>
<td><p>Actualizado</p></td>
<td><p>Tanto el cliente SMB 3.0 como el servidor SMB 3.0 se han optimizado para las E/S de lectura/escritura pequeñas y aleatorias, que son comunes en las aplicaciones de servidores como SQL Server OLTP. Además, la unidad de transmisión máxima (MTU) grande se enciende de manera predeterminada, lo que mejora el rendimiento de manera significativa en las grandes transferencias secuenciales, como el almacenamiento de datos de SQL Server, las restauraciones o copias de seguridad de bases de datos, la implementación o las copias de los discos duros virtuales.</p></td>
</tr>
<tr class="odd">
<td><p>Cmdlets de Windows PowerShell específicos de SMB</p></td>
<td><p>Nuevo</p></td>
<td><p>Con los cmdlets de Windows PowerShell para SMB, un administrador puede administrar los recursos compartidos de archivos en servidores de archivos, de un extremo a otro, desde la línea de comandos.</p></td>
</tr>
<tr class="even">
<td><p>Cifrado SMB</p></td>
<td><p>Nuevo</p></td>
<td><p>Proporciona cifrados de datos SMB de un extremo a otro y proyecta los datos desde las ocurrencias de interceptación de redes que no son de confianza. No requiere de costos de implementación nuevos y no se necesita del protocolo de seguridad de Internet (IPsec), hardware especializados ni aceleradores de WAN. Puede configurarse por recurso compartido o para todo el servidor de archivos y puede habilitarse para una variedad de escenarios donde los datos se envían a través de redes que no son de confianza.</p></td>
</tr>
<tr class="odd">
<td><p>Concesión de directorio SMB</p></td>
<td><p>Nuevo</p></td>
<td><p>Mejora los tiempos de respuesta de las aplicaciones en las sucursales. Con el uso de las concesiones de directorio, los recorridos de ida y vuelta desde el cliente al servidor se reducen porque los metadatos se recuperan desde una caché de directorio de mayor vida útil. Se mantiene la coherencia de la memoria caché porque se notifica a los clientes cuando realizan modificaciones en la información del directorio del servidor. Funciona con escenarios para <em>CarpetaDeInicio</em> (lectura/escritura sin uso compartido) y <em>Publicación</em> (solo lectura con uso compartido).</p></td>
</tr>
</tbody>
</table>

## <a name="hardware-requirements"></a>Requisitos de hardware

La conmutación por error transparente SMB tiene los siguientes requisitos:

* Un clúster de conmutación por error que ejecute Windows Server 2012 o Windows Server 2016 con al menos dos nodos configurados. El clúster debe aprobar las pruebas de validación de clúster incluidas en el asistente de validación.
* Los recursos compartidos de archivos deben estar creados con la propiedad de disponibilidad constante (CA), que es la predeterminada.
* Los recursos compartidos de archivos deben estar creados en rutas de volumen CSV para alcanzar la escalabilidad horizontal SMB.
* Los equipos cliente deben ejecutar Windows® 8 o Windows Server 2012, y ambos incluyen el cliente SMB actualizado que admite la disponibilidad continua.

>[!NOTE]
>Los clientes de nivel inferior pueden conectarse a recursos compartidos de archivos que tienen la propiedad CA, pero no se admite la conmutación por error transparente para estos clientes.

SMB Multichannel tiene los siguientes requisitos:

* Se requieren al menos dos equipos que ejecuten Windows Server 2012. No es necesario instalar características adicionales, ya que la tecnología está habilitada de manera predeterminada.
* Para obtener información sobre las configuraciones de red recomendadas, consulte la sección Consulte también al final de este tema de descripción general.

SMB Direct tiene los siguientes requisitos:

* Se requieren al menos dos equipos que ejecuten Windows Server 2012. No es necesario instalar características adicionales, ya que la tecnología está habilitada de manera predeterminada.
* Se requieren de adaptadores de red con capacidades RDMA. Actualmente, estos adaptadores están disponibles en tres tipos diferentes: iWARP, Infiniband o RoCE (RDMA a través de redes Ethernet convergidas).

## <a name="more-information"></a>Más información

En la siguiente lista se proporcionan recursos adicionales en la web sobre SMB y tecnologías relacionadas en Windows Server 2012 R2, Windows Server 2012 y Windows Server 2016.

* [Almacenamiento en Windows Server](../storage.md)
* [Servidor de archivos de escalabilidad horizontal para los datos de la aplicación](../../failover-clustering/sofs-overview.md)
* [Mejorar el rendimiento de un servidor de archivos con SMB directo](smb-direct.md)
* [Implementación de Hyper-V en SMB](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/jj134187(v%3dws.11)>)
* [Implementación de SMB multicanal](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/dn610980(v%3dws.11)>)
* [Implementación de servidores de archivos rápidos y eficientes para aplicaciones de servidor](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/hh831723(v%3dws.11)>)
* [SMB: Guía de solución de problemas](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/dn659439(v%3dws.11)>)