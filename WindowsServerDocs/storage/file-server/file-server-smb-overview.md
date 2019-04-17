---
title: Información general de uso compartido de archivos mediante el protocolo SMB 3 de Windows Server
description: Una descripción general de usar el protocolo de SMB 3 para recursos compartidos de archivos y file servers con Windows Server.
ms.prod: windows-server-threshold
ms.topic: article
author: JasonGerend
ms.author: jgerend
ms.technology: storage
ms.date: 07/09/2018
ms.localizationpriority: medium
ms.openlocfilehash: fc4c8b341ee78db80f862ee412400f0a930fe810
ms.sourcegitcommit: 375e94dc70c76e7aa5679c32f0f4dbc26cf7dd83
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/10/2018
ms.locfileid: "2233491"
---
# <a name="overview-of-file-sharing-using-the-smb-3-protocol-in-windows-server"></a>Información general de uso compartido de archivos mediante el protocolo SMB 3 de Windows Server

>Se aplica a: Windows Server 2012 R2, Windows Server 2012, Windows Server 2016

Este tema describe la característica de SMB 3.0 en Windows Server® 2012, Windows Server 2012 R2 y Windows Server 2016 — práctico usa para la característica, los más importantes de nuevo o actualizado la funcionalidad de esta versión en comparación con versiones anteriores y el hardware requisitos.

## <a name="feature-description"></a>Descripción de la característica

El protocolo del Bloque de mensajes del servidor (SMB) es un protocolo de uso compartido de archivos de red que permite que las aplicaciones de un equipo puedan leer y escribir archivos y solicitar servicios desde los programas de un servidor en una red de equipos. El protocolo SMB puede usarse sobre el protocolo TCP/IP u otros protocolos de red. Con el uso de un protocolo SMB, una aplicación (o el usuario de una aplicación) puede acceder a los archivos u otros recursos de un servidor remoto. Esto permite que las aplicaciones puedan leer, crear y actualizar archivos en un servidor remoto. También puedes comunicarse con cualquier programa del servidor que esté configurado para recibir la solicitud de un cliente SMB. Windows Server 2012 presenta la nueva versión 3.0 del protocolo SMB.

## <a name="practical-applications"></a>Aplicaciones prácticas

Esta sección describen algunas formas prácticas nuevo para usar el nuevo protocolo de SMB 3.0.

* **Almacenamiento de archivos de virtualización (Hyper-V™ a través de SMB)**. Hyper-V puede almacenar los archivos de máquina virtual, como la configuración, los archivos de disco duro (VHD) de Virtual e instantáneas, en recursos compartidos de archivos a través del protocolo SMB 3.0. Esto se puede usar para servidores de archivos independiente y servidores de archivos en clúster que usar Hyper-V junto con el almacenamiento de archivos compartidos para el clúster.
* **Microsoft SQL Server a través de SMB**. SQL Server puede almacenar los archivos de base de datos de usuario en recursos compartidos de archivos SMB. Actualmente, esto es compatible con SQL Server 2008 R2 para los servidores independientes de SQL. Próximas versiones de SQL Server va a agregar compatibilidad con servidores agrupados en clúster de SQL y las bases de datos del sistema.
* **Almacenamiento de información tradicional para los datos del usuario final**. El protocolo SMB 3.0 proporciona mejoras para el trabajador de la información (o cliente) de las cargas de trabajo. Estas mejoras incluyen la reducción de las latencias de aplicación experimentadas por los usuarios de sucursales al obtener acceso a datos a través de redes de área extensa (WAN) y la protección de datos intercepten los ataques.

## <a name="new-and-changed-functionality"></a>Funcionalidad nueva y modificada

Para obtener información sobre las funciones nuevas y modificadas en Windows Server 2012 R2, consulte [What ' s New in SMB en Windows Server](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/hh831474(v%3dws.11)>).

SMB en Windows Server 2012 y Windows Server 2016 incluye el nuevo protocolo de SMB 3.0 y muchas mejoras nuevas que se describen en la siguiente tabla.

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
<td><p>Conmutación por error transparente de SMB</p></td>
<td><p>Nuevo</p></td>
<td><p>Permite a los administradores realizar el mantenimiento de hardware o software de nodos en un servidor de archivos agrupado sin interrumpir el almacenamiento de datos en estos recursos compartidos de archivos de aplicaciones de servidor. Además, si se produce un error de hardware o software en un nodo de clúster, los clientes SMB transparente volver a conectar a otro nodo de clúster sin interrumpa la ejecución de aplicaciones de servidor que almacenan datos en estos recursos compartidos de archivos.</p></td>
</tr>
<tr class="even">
<td><p>Escalabilidad horizontal de SMB</p></td>
<td><p>Nuevo</p></td>
<td><p>Uso de volúmenes compartidos clúster (CSV) versión 2, los administradores pueden crear recursos compartidos de archivos que proporcionan acceso simultáneo a archivos de datos, con E/S directa, a través de todos los nodos de un clúster de servidores de archivo. Esto proporciona una mejor utilización de ancho de banda de red y el equilibrio de carga de los clientes de servidor de archivo y optimiza el rendimiento para aplicaciones de servidor.</p></td>
</tr>
<tr class="odd">
<td><p>Multicanal de SMB</p></td>
<td><p>Nuevo</p></td>
<td><p>Permite la agregación de ancho de banda de red y tolerancia a errores de red si existen varias rutas de acceso entre el cliente de SMB 3.0 y el servidor SMB 3.0. Esto permite que las aplicaciones de servidor aprovechar al máximo de todos los ancho de banda de red disponible y ser resistente a un error de red.</p></td>
</tr>
<tr class="even">
<td><p>Directo de SMB</p></td>
<td><p>Nuevo</p></td>
<td><p>Admite el uso de adaptadores de red que tienen la capacidad de RDMA y puede funcionar a la velocidad máxima con una latencia muy baja, durante el uso de CPU muy poco. Para las cargas de trabajo como Hyper-V o Microsoft SQL Server, esto permite que un servidor de archivos remoto similar a la de almacenamiento local.</p></td>
</tr>
<tr class="odd">
<td><p>Contadores de rendimiento para aplicaciones de servidor</p></td>
<td><p>Nuevo</p></td>
<td><p>El rendimiento de SMB nuevos contadores proporcionan detallados, por-compartir información sobre el rendimiento, la latencia y la E/S por segundo (IOPS), lo que permite a los administradores a analizar el rendimiento de los recursos compartidos de archivos de SMB 3.0 donde se almacenan sus datos. Estos contadores están diseñados específicamente para aplicaciones de servidor, como Hyper-V y SQL Server, que almacenar los archivos en recursos compartidos de archivos remotos.</p></td>
</tr>
<tr class="even">
<td><p>Optimizaciones de rendimiento</p></td>
<td><p>Actualizado</p></td>
<td><p>El cliente SMB 3.0 y el servidor SMB 3.0 se han optimizado para E/S de lectura/escritura aleatoria pequeñas, que es común en las aplicaciones de servidor como OLTP de SQL Server. Además, los grandes unidad de transmisión máxima (MTU) está activado de forma predeterminada, lo que mejora considerablemente el rendimiento en grandes transferencias secuenciales, como almacén de datos de SQL Server, la copia de seguridad de base de datos o la restauración, implementación o copiar discos duros virtuales.</p></td>
</tr>
<tr class="odd">
<td><p>Cmdlets de Windows PowerShell específicos de SMB</p></td>
<td><p>Nuevo</p></td>
<td><p>Con los cmdlets de Windows PowerShell para SMB, un administrador puede administrar los recursos compartidos de archivos en el servidor de archivos, a fin de otra, desde la línea de comandos.</p></td>
</tr>
<tr class="even">
<td><p>Cifrado de SMB</p></td>
<td><p>Nuevo</p></td>
<td><p>Proporciona cifrado de extremo a otro de los datos SMB y protege los datos de interceptación repeticiones en redes que no se confía. Requiere ningún nuevos costos de implementación y no hay necesidad de seguridad (IPsec) de protocolo de Internet, hardware especializado o aceleradores de WAN. Puede configurarse de forma por compartir, o para el servidor de archivos completo y se puede habilitar para una variedad de escenarios donde datos atraviesan redes que no se confía.</p></td>
</tr>
<tr class="odd">
<td><p>Leasing de directorios</p></td>
<td><p>Nuevo</p></td>
<td><p>Mejora los tiempos de respuesta de la aplicación en las sucursales. Con el uso de concesiones de Active directory, ida y vuelta desde el cliente al servidor se reduce, ya que se han recuperado metadatos de un mayor vida memoria caché de Active directory. Coherencia de caché se mantiene debido a que los clientes reciben notificaciones cuando cambia la información de Active directory en el servidor. Funciona con escenarios de <em>publicación</em> (solo lectura con el uso compartido) y <em>HomeFolder</em> (lectura y escritura con ningún uso compartido).</p></td>
</tr>
</tbody>
</table>

## <a name="hardware-requirements"></a>Requisitos de hardware

Conmutación por error transparente de SMB tiene los siguientes requisitos:

* Un clúster de conmutación por error que ejecuta Windows Server 2012 o Windows Server 2016 con al menos dos nodos configurados. El clúster debe pasar las pruebas de validación de clúster incluidas en el Asistente para validación.
* Recursos compartidos de archivos deben crearse con la propiedad disponibilidad continua (CA), que es el valor predeterminado.
* Recursos compartidos de archivos deben crearse en rutas de acceso de volumen CSV a la consecución de escalado horizontal SMB.
* Los equipos cliente deben ejecutar Windows® 8 o Windows Server 2012, ambos de los cuales incluyen al cliente SMB actualizado que admita la disponibilidad continua.

>[!NOTE]
>Los clientes de nivel inferior pueden conectarse a recursos compartidos de archivos que tienen la propiedad de entidad emisora de certificados, pero no se admite la conmutación por error transparente para estos clientes.

Multicanal de SMB tiene los siguientes requisitos:

* Se requieren al menos dos equipos que ejecutan Windows Server 2012. No hay características adicionales que deban instalarse: la tecnología está en de forma predeterminada.
* Para obtener información sobre las configuraciones de red recomendadas, vea la sección Vea también al final de este tema de información general.

Directo de SMB tiene los siguientes requisitos:

* Se requieren al menos dos equipos que ejecutan Windows Server 2012. No hay características adicionales que deban instalarse: la tecnología está en de forma predeterminada.
* Adaptadores de red con capacidad de RDMA son necesarios. Actualmente, estos adaptadores están disponibles en tres tipos diferentes: iWARP, Infiniband o RoCE (RDMA a través de Ethernet ha convergido).

## <a name="more-information"></a>Más información

En la siguiente lista se proporciona recursos adicionales en Internet sobre SMB y tecnologías relacionadas en Windows Server 2012 R2, Windows Server 2012 y Windows Server 2016.

* [Almacenamiento en Windows Server](../storage.md)
* [Servidor de archivos de escalado horizontal para datos de la aplicación](../../failover-clustering/sofs-overview.md)
* [Mejorar el rendimiento de un servidor de archivos con SMB directa](smb-direct.md)
* [Implementar Hyper-V en SMB](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/jj134187(v%3dws.11)>)
* [Implementar multicanal de SMB](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/dn610980(v%3dws.11)>)
* [Implementación de servidores de archivos rápida y eficaz para aplicaciones de servidor](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/hh831723(v%3dws.11)>)
* [SMB: Guía de solución de problemas](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/dn659439(v%3dws.11)>)