---
title: Información general sobre el uso compartido de archivos mediante el protocolo SMB 3 en Windows Server
description: Información general sobre el uso del protocolo SMB 3 para recursos compartidos de archivos y archivos con Windows Server.
ms.prod: windows-server
ms.topic: article
author: JasonGerend
ms.author: jgerend
ms.technology: storage
ms.date: 01/10/2020
ms.localizationpriority: medium
ms.openlocfilehash: 416145a8c4ec20eaf46cf4b5ac88a0cdf38bdf33
ms.sourcegitcommit: 76469d1b7465800315eaca3e0c7f0438fc3939ed
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/13/2020
ms.locfileid: "75919885"
---
# <a name="overview-of-file-sharing-using-the-smb-3-protocol-in-windows-server"></a>Información general sobre el uso compartido de archivos mediante el protocolo SMB 3 en Windows Server

>Se aplica a: Windows Server 2019, Windows Server 2016, Windows Server 2012 R2 y Windows Server 2012

En este tema se describe la característica SMB 3 en Windows Server 2019, Windows Server 2016, Windows Server 2012 R2 y Windows Server 2012: usos prácticos para la característica, la funcionalidad nueva o actualizada más importante de esta versión en comparación con las versiones anteriores. los requisitos de hardware. SMB es también un protocolo de tejido usado por soluciones [del centro de datos definido por software (SDDC)](../../sddc.md) como espacios de almacenamiento directo, réplica de almacenamiento y otros. La versión 3,0 de SMB se presentó con Windows Server 2012 y se ha mejorado de forma incremental en las versiones posteriores.

## <a name="feature-description"></a>Descripción de la característica

El protocolo del Bloque de mensajes del servidor (SMB) es un protocolo de uso compartido de archivos de red que permite que las aplicaciones de un equipo puedan leer y escribir archivos y solicitar servicios desde los programas de un servidor en una red de equipos. El protocolo SMB puede usarse sobre el protocolo TCP/IP u otros protocolos de red. Con el uso de un protocolo SMB, una aplicación (o el usuario de una aplicación) puede acceder a los archivos u otros recursos de un servidor remoto. Esto permite que las aplicaciones puedan leer, crear y actualizar archivos en un servidor remoto. SMB también puede comunicarse con cualquier programa de servidor que esté configurado para recibir una solicitud de cliente SMB. SMB es un protocolo de tejido que usan las tecnologías de computación del centro de datos definido por software (SDDC), como Espacios de almacenamiento directo, réplica de almacenamiento. Para obtener más información, consulte [Windows Server Software-Defined Datacenter](../../sddc.md).

## <a name="practical-applications"></a>Aplicaciones prácticas

En esta sección se tratan algunas nuevas maneras prácticas para usar el nuevo protocolo SMB 3.0.

* **Almacenamiento de archivos para visualización (Hyper-V™ a través de SMB)** . En Hyper-V se pueden almacenar archivos de máquinas virtuales, como los archivos de configuración, disco duro virtual (VHD) e instantáneas, en recursos de archivos compartidos a través del protocolo SMB 3.0. Esto puede usarse tanto para los servidores de archivos independientes como para los servidores de archivos agrupados que usan Hyper-V con el almacenamiento de archivos compartidos del clúster.
* **Microsoft SQL Server a través de SMB**. En SQL Server se pueden almacenar archivos de base de datos de usuarios en recursos compartidos de archivos SMB. Actualmente, esto se admite con SQL Server 2008 R2 para los servidores SQL independientes. Las próximas versiones de SQL Server admitirán servidores SQL Server en clúster y bases de datos del sistema.
* **Almacenamiento tradicional para datos de usuario final**. El protocolo SMB 3.0 cuenta con mejoras para las cargas de trabajo del trabajador de la información (o cliente). Estas mejoras incluyen la reducción de las latencias de la aplicación que experimentan los usuarios de las sucursales cuando acceden a los datos a través de las redes de área extensa (WAN) y protegen los datos contra los ataques de interceptación.

## <a name="new-and-changed-functionality"></a>Funcionalidad nueva y modificada

En las secciones siguientes se describe la funcionalidad que se agregó en SMB 3 y las actualizaciones posteriores.

## <a name="features-added-in-windows-server-2019-and-windows-10-version-1809"></a>Características agregadas en Windows Server 2019 y Windows 10, versión 1809

| Característica/funcionalidad  | Nueva o actualizada  | Resumen  |
| --------- | --------- | --------- |
| Capacidad de requerir la escritura a través del disco en recursos compartidos de archivos que no están disponibles continuamente | New | Para proporcionar una seguridad adicional que escriba en un recurso compartido de archivos para que sea todo el camino a través de la pila de software y hardware hasta el disco físico antes de que la operación de escritura se devuelva como completada, puede habilitar la escritura a través en el recurso compartido de archivos mediante el comando `NET USE /WRITETHROUGH` del cmdlet de PowerShell `New-SMBMapping -UseWriteThrough`. El rendimiento se ve afectado por el uso de la escritura a través. Vea la entrada de blog sobre el [control de los comportamientos de escritura en SMB](https://techcommunity.microsoft.com/t5/storage-at-microsoft/controlling-write-through-behaviors-in-smb/bc-p/1083417#M677) para más información. |

## <a name="features-added-in-windows-server-version-1709-and-windows-10-version-1709"></a>Características agregadas en Windows Server, versión 1709 y Windows 10, versión 1709

| Característica/funcionalidad  | Nueva o actualizada  | Resumen  |
| --------- | --------- | --------- |
| El acceso de invitado a recursos compartidos de archivos está deshabilitado | New | El cliente SMB ya no permite las siguientes acciones: acceso a la cuenta de invitado a un servidor remoto. Reserva a la cuenta de invitado después de proporcionar credenciales no válidas. Para obtener más información, consulte [acceso de invitado en SMB2 deshabilitado de forma predeterminada en Windows](https://support.microsoft.com/help/4046019/guest-access-in-smb2-disabled-by-default-in-windows-10-and-windows-ser). | 
| Asignación global de SMB | New | Asigna un recurso compartido SMB remoto a una letra de unidad a la que pueden tener acceso todos los usuarios del host local, incluidos los contenedores. Esto es necesario para habilitar la e/s de contenedor en el volumen de datos para atravesar el punto de montaje remoto. Tenga en cuenta que cuando se usa la asignación global SMB para contenedores, todos los usuarios del host de contenedor pueden tener acceso al recurso compartido remoto. Cualquier aplicación que se ejecute en el host de contenedor también tendrá acceso al recurso compartido remoto asignado. Para obtener más información, consulte [compatibilidad de almacenamiento de contenedor con volúmenes compartidos de clúster (CSV), espacios de almacenamiento directo, asignación global de SMB](https://techcommunity.microsoft.com/t5/failover-clustering/container-storage-support-with-cluster-shared-volumes-csv/ba-p/372140). |
| Control de dialecto SMB | New | Ahora puede establecer los valores del registro para controlar la versión mínima de SMB (dialecto) y la versión máxima de SMB utilizada. Para obtener más información, vea [controlar los dialectos SMB](https://techcommunity.microsoft.com/t5/storage-at-microsoft/controlling-smb-dialects/ba-p/860024). |

## <a name="features-added-in-smb-311-with-windows-server-2016-and-windows-10-version-1607"></a>Características agregadas en SMB 3,11 con Windows Server 2016 y Windows 10, versión 1607

| Característica/funcionalidad  | Nueva o actualizada  | Resumen  |
| --------- | --------- | --------- |
| Cifrado SMB     |   Actualizado      | El cifrado de SMB 3.1.1 con Estándar de cifrado avanzado-Galois/Counter (AES-GCM) es más rápido que la firma SMB o el cifrado SMB anterior mediante AES-CCM.   |
| Almacenamiento en caché de directorio | New | SMB 3.1.1 incluye mejoras en el almacenamiento en caché del directorio. Los clientes de Windows ahora pueden almacenar en caché directorios mucho más grandes, aproximadamente 500 000 entradas. Los clientes de Windows intentarán realizar consultas de directorio con búferes de 1 MB para reducir los viajes de ida y vuelta y mejorar el rendimiento. |
| Integridad de autenticación previa | New |  En SMB 3.1.1, la integridad de la autenticación previa proporciona una protección mejorada frente a ataques de tipo "Man in the Middle" que manipulan los mensajes de autenticación y el establecimiento de conexiones de SMB. Para obtener más información, consulte [la integridad de la autenticación previa de SMB 3.1.1 en Windows 10](https://docs.microsoft.com/archive/blogs/openspecification/smb-3-1-1-pre-authentication-integrity-in-windows-10). |
| Mejoras de cifrado de SMB | New | SMB 3.1.1 ofrece un mecanismo para negociar el algoritmo criptográfico por conexión, con opciones para AES-128-CCM y AES-128-GCM. AES-128-GCM es el valor predeterminado para las nuevas versiones de Windows, mientras que las versiones anteriores seguirán usando AES-128-CCM. |
| Compatibilidad con la actualización gradual de clúster | New | Permite actualizar las [actualizaciones del clúster](../../failover-clustering/cluster-operating-system-rolling-upgrade.md) permitiendo que SMB parezca ser compatible con versiones máximas de SMB para clústeres en el proceso de actualización. Para obtener más detalles sobre cómo permitir que SMB se comunique con diferentes versiones (dialectos) del Protocolo, vea la entrada de blog sobre el [control de dialectos SMB](https://techcommunity.microsoft.com/t5/storage-at-microsoft/controlling-smb-dialects/ba-p/860024). |
| Compatibilidad con clientes de SMB directo en Windows 10 | New | Windows 10 Enterprise, Windows 10 Education y Windows 10 Pro for Workstation ahora incluyen compatibilidad con clientes de SMB directo. |
| Compatibilidad nativa con llamadas API de FileNormalizedNameInformation | New | Agrega compatibilidad nativa para consultar el nombre normalizado de un archivo. Para obtener más información, consulte [FileNormalizedNameInformation](https://docs.microsoft.com/openspecs/windows_protocols/ms-fscc/20bcadba-808c-4880-b757-4af93e41edf6). |

Para obtener más información, consulte la entrada de blog [novedades de SMB 3.1.1 en Windows Server 2016 Technical Preview 2](https://docs.microsoft.com/archive/blogs/josebda/whats-new-in-smb-3-1-1-in-the-windows-server-2016-technical-preview-2).

## <a name="features-added-in-smb-302-with-windows-server-2012-r2-and-windows-81"></a>Características agregadas en SMB 3,02 con Windows Server 2012 R2 y Windows 8.1

| Característica/funcionalidad  | Nueva o actualizada  | Resumen  |
| --------- | --------- | --------- |
| Nuevo equilibrio automático de clientes del servidor de archivos de escalabilidad horizontal     |   New      | Mejora la escalabilidad y la capacidad de administración de los servidores de archivos de escalabilidad horizontal. El seguimiento de las conexiones de cliente SMB se realiza por recurso compartido de archivos (en lugar de por servidor) y, a continuación, se redirige a los clientes al nodo del clúster con el mejor acceso al volumen usado por el recurso compartido de archivos. Esto mejora la eficiencia al reducir el tráfico de redirección entre los nodos del servidor de archivos. Los clientes se redirigen tras una conexión inicial y cuando se vuelve a configurar el almacenamiento de clúster.    |
| Rendimiento a través de WAN   | Actualizado  | Windows 8.1 y Windows 10 proporcionan mejoras en la compatibilidad con CopyFile SRV_COPYCHUNK sobre SMB al usar el explorador de archivos para copias remotas de una ubicación de un equipo remoto a otra copia en el mismo servidor. Solo copiará una pequeña cantidad de metadatos a través de la red (se transmite 1/2KiB por 16MiB de datos de archivo). Esto da como resultado una mejora significativa del rendimiento. Se trata de una distinción en el nivel de sistema operativo y en el explorador de archivos para SMB. |
| SMB directo     |   Actualizado      | Mejora el rendimiento de pequeñas cargas de trabajo de E/S mediante el aumento de la eficiencia al hospedar cargas de trabajo con operaciones de E/S pequeñas (como una base de datos de procesamiento de transacciones en línea, u OLTP, en una máquina virtual). Estas mejoras son evidentes cuando se usan interfaces de red de mayor velocidad, como Ethernet 40 Gbps y 56 Gbps InfiniBand.  |
| Límites de ancho de banda de SMB | New | Ahora puede usar [set-SmbBandwidthLimit](https://docs.microsoft.com/powershell/module/smbshare/set-smbbandwidthlimit) para establecer límites de ancho de banda en tres categorías: VirtualMachine (Hyper-v a través del tráfico SMB), LiveMigration (Hyper-v migración en vivo tráfico a través de SMB) o predeterminado (todos los demás tipos de tráfico SMB).

Para obtener más información sobre la funcionalidad SMB nueva y modificada en Windows Server 2012 R2, consulte [novedades de SMB en Windows Server](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/hh831474(v%3dws.11)>).

## <a name="features-added-in-smb-30-with-windows-server-2012-and-windows-8"></a>Características agregadas en SMB 3,0 con Windows Server 2012 y Windows 8

| Característica/funcionalidad  | Nueva o actualizada  | Resumen  |
| --------- | --------- | --------- |
| Conmutación por error transparente SMB     |   New    | Permite que los administradores puedan realizar mantenimientos de hardware o software de los nodos en un servidor de archivos en clúster sin interrumpir las aplicaciones del servidor que almacenan datos en estos recursos compartidos de archivos. Además, si ocurre un error de hardware o software en el nodo de clúster, los clientes SMB se vuelven a conectar de forma transparente en otro nodo de clúster sin interrumpir las aplicaciones de servidor que están almacenando datos en estos recursos compartidos de archivos.        |
| Escalabilidad horizontal SMB     |   New      | Compatibilidad con varias instancias SMB en una Servidor de archivos de escalabilidad horizontal. Al usar volúmenes compartidos de clúster (CSV) versión 2, los administradores pueden crear recursos compartidos de archivos que proporcionan acceso simultáneo a los archivos de datos, con E/S directa, a través de todos los nodos de un clúster de servidores de archivos. Esto proporciona una mejor utilización del ancho de banda de red y el equilibrio de carga de los clientes de servidores de archivos y optimiza el rendimiento de las aplicaciones de los servidores.  |
| SMB multicanal     |   New      |  Habilita la agregación de ancho de banda de red y tolerancia a errores de red si hay varias rutas de acceso disponibles entre el cliente SMB y el servidor. Esto permite que las aplicaciones de servidores aprovechen todo el ancho de banda de red disponible y sean resistentes a los errores de red.<br><br>SMB multicanal en SMB 3 contribuye a un aumento sustancial del rendimiento en comparación con las versiones anteriores de SMB. |
| SMB directo     |   New      | Admite el uso de adaptadores de red que tienen capacidades RDMA y pueden funcionar a toda velocidad con una latencia muy baja y, al mismo tiempo, minimizan el uso de CPU. Para las cargas de trabajo como Hyper-V o Microsoft SQL Server, esto permite que un servidor de archivo remoto se parezca al almacenamiento local.<br><br>SMB directo en SMB 3 contribuye a un aumento sustancial del rendimiento en comparación con las versiones anteriores de SMB.  |
| Contadores de rendimiento para las aplicaciones de servidores     |   New      |  Los nuevos contadores de rendimiento de SMB proporcionan información detallada, por recurso compartido sobre el rendimiento, la latencia y la e/s por segundo (IOPS), lo que permite a los administradores analizar el rendimiento de los recursos compartidos de archivos SMB en los que se almacenan sus datos. Estos contadores están especialmente diseñados para las aplicaciones de servidores, como Hyper-V y SQL Server, que almacenan archivos en los recursos compartidos de archivos remotos.      |
| Optimizaciones de rendimiento    |  Actualizado   | Tanto el cliente SMB como el servidor se han optimizado para operaciones de e/s de lectura y escritura aleatorias pequeñas, que es habitual en aplicaciones de servidor como SQL Server OLTP. Además, la unidad de transmisión máxima (MTU) grande se enciende de manera predeterminada, lo que mejora el rendimiento de manera significativa en las grandes transferencias secuenciales, como el almacenamiento de datos de SQL Server, las restauraciones o copias de seguridad de bases de datos, la implementación o las copias de los discos duros virtuales. |
| Cmdlets de Windows PowerShell específicos de SMB     |   New      |  Con los cmdlets de Windows PowerShell para SMB, un administrador puede administrar los recursos compartidos de archivos en servidores de archivos, de un extremo a otro, desde la línea de comandos.   |
| Cifrado SMB     |   New      | Proporciona cifrados de datos SMB de un extremo a otro y proyecta los datos desde las ocurrencias de interceptación de redes que no son de confianza. No requiere de costos de implementación nuevos y no se necesita del protocolo de seguridad de Internet (IPsec), hardware especializados ni aceleradores de WAN. Puede configurarse por recurso compartido o para todo el servidor de archivos y puede habilitarse para una variedad de escenarios donde los datos se envían a través de redes que no son de confianza. |
| Concesión de directorio SMB     |  New | Mejora los tiempos de respuesta de las aplicaciones en las sucursales. Con el uso de las concesiones de directorio, los recorridos de ida y vuelta desde el cliente al servidor se reducen porque los metadatos se recuperan desde una caché de directorio de mayor vida útil. Se mantiene la coherencia de la memoria caché porque se notifica a los clientes cuando realizan modificaciones en la información del directorio del servidor. Las concesiones de directorio funcionan con escenarios para Carpetadeinicio (lectura/escritura sin uso compartido) y publicación (solo lectura con uso compartido).    |
| Rendimiento a través de WAN   | New   | Los bloqueos oportunistas del directorio (bloqueos oportunistas) y las concesiones de bloqueo oportunista se introdujeron en SMB 3,0. En el caso de cargas de trabajo de Office/cliente típicas, se muestran bloqueos oportunistas/concesiones para reducir los recorridos de ida y vuelta de red en un 15% aproximadamente.<br><br>En SMB 3, la implementación de Windows de SMB se ha perfeccionado para mejorar el comportamiento de almacenamiento en caché en el cliente, así como la capacidad de introducir mayor rendimiento.<br><br>SMB 3 incorpora mejoras a la API CopyFile (), así como a herramientas asociadas como Robocopy, para introducir significativamente más datos a través de la red. |
| Negociación segura de dialectos | New | Ayuda a protegerse contra el intento de cambio de tipo "Man in the Middle" para degradar la negociación del dialecto. La idea es evitar que un atacante pueda degradar el dialecto y las capacidades negociadas inicialmente entre el cliente y el servidor. Para obtener más información, vea [SMB3 Secure Dialect](https://docs.microsoft.com/archive/blogs/openspecification/smb3-secure-dialect-negotiation). Tenga en cuenta que esto se ha sustituido por la característica de [integridad de autenticación previa de SMB 3.1.1 en Windows 10](https://docs.microsoft.com/archive/blogs/openspecification/smb-3-1-1-pre-authentication-integrity-in-windows-10) en SMB 3.1.1. |


## <a name="hardware-requirements"></a>Requisitos de hardware

La conmutación por error transparente SMB tiene los siguientes requisitos:

* Un clúster de conmutación por error que ejecute Windows Server 2012 o Windows Server 2016 con al menos dos nodos configurados. El clúster debe aprobar las pruebas de validación de clúster incluidas en el asistente de validación.
* Los recursos compartidos de archivos deben estar creados con la propiedad de disponibilidad constante (CA), que es la predeterminada.
* Los recursos compartidos de archivos deben estar creados en rutas de volumen CSV para alcanzar la escalabilidad horizontal SMB.
* Los equipos cliente deben ejecutar Windows® 8 o Windows Server 2012, y ambos incluyen el cliente SMB actualizado que admite la disponibilidad continua.

> [!NOTE]
> Los clientes de nivel inferior pueden conectarse a recursos compartidos de archivos que tienen la propiedad CA, pero no se admite la conmutación por error transparente para estos clientes.

SMB Multichannel tiene los siguientes requisitos:

* Se requieren al menos dos equipos que ejecuten Windows Server 2012. No es necesario instalar características adicionales, ya que la tecnología está habilitada de manera predeterminada.
* Para obtener información sobre las configuraciones de red recomendadas, consulte la sección Consulte también al final de este tema de descripción general.

SMB Direct tiene los siguientes requisitos:

* Se requieren al menos dos equipos que ejecuten Windows Server 2012. No es necesario instalar características adicionales, ya que la tecnología está habilitada de manera predeterminada.
* Se requieren de adaptadores de red con capacidades RDMA. Actualmente, estos adaptadores están disponibles en tres tipos diferentes: iWARP, Infiniband o RoCE (RDMA a través de redes Ethernet convergidas).

## <a name="more-information"></a>Información adicional

En la siguiente lista se proporcionan recursos adicionales en la web sobre SMB y tecnologías relacionadas en Windows Server 2012 R2, Windows Server 2012 y Windows Server 2016.

* [Almacenamiento en Windows Server](../storage.md)
* [Servidor de archivos de escalabilidad horizontal para los datos de la aplicación](../../failover-clustering/sofs-overview.md)
* [Mejorar el rendimiento de un servidor de archivos con SMB directo](smb-direct.md)
* [Implementación de Hyper-V en SMB](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/jj134187(v%3dws.11)>)
* [Implementación de SMB multicanal](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/dn610980(v%3dws.11)>)
* [Implementación de servidores de archivos rápidos y eficientes para aplicaciones de servidor](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/hh831723(v%3dws.11)>)
* [SMB: Guía de solución de problemas](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/dn659439(v%3dws.11)>)
