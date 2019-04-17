---
title: Servidor de archivos de escalado horizontal para obtener información general de datos de aplicación
description: Información general de la característica de servidor de archivos de escalado horizontal para Windows Server 201 R2, Windows Server 2012 y Windows Server 2016.
ms.prod: windows-server-threshold
ms.topic: article
author: JasonGerend
ms.author: jgerend
ms.technology: storage-failover-clustering
ms.date: 04/26/2018
ms.localizationpriority: medium
ms.openlocfilehash: 04e25e9c69062611d9d14c220614f148ac5de770
ms.sourcegitcommit: e0479b0114eac7f232e8b1e45eeede96ccd72b26
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 06/22/2018
ms.locfileid: "2082670"
---
# <a name="scale-out-file-server-for-application-data-overview"></a>Servidor de archivos de escalado horizontal para obtener información general de datos de aplicación

>Se aplica a: Windows Server 2012 R2, Windows Server 2012, Windows Server 2016

Servidor de archivos de escalado horizontal es una característica que está diseñada para proporcionar recursos compartidos de archivos de escalado horizontal que están continuamente disponibles para el almacenamiento de la aplicación de servidor basado en el archivo. Recursos compartidos de archivos de escalado horizontal proporciona la capacidad de compartir la misma carpeta de varios nodos del mismo clúster. En este escenario se centra en cómo planear e implementar el servidor de archivos de escalado horizontal.

Puede implementar y configurar un servidor de archivos agrupado mediante cualquiera de los métodos siguientes:

- **Escalado horizontal de servidor de archivos de datos de aplicación** Esta característica de servidor de archivos agrupado se introdujo en Windows Server 2012, y le permite almacenar datos de aplicación de servidor, como los archivos de máquina virtual de Hyper-V, en recursos compartidos de archivos y obtener un nivel de confiabilidad, disponibilidad, capacidad de administración y alta similar rendimiento que cabría esperar de una red de área de almacenamiento. Todos los recursos compartidos de archivos están simultáneamente en línea en todos los nodos. Recursos compartidos de archivos asociados con este tipo de servidor de archivos agrupado se denominan recursos compartidos de archivos de escalado horizontal. A veces se conoce como activa. Éste es el tipo de servidor de archivos que se recomienda al implementar cualquiera Hyper-V a través de bloque de mensajes del servidor (SMB) o Microsoft SQL Server a través de SMB.
- **Servidor de archivos para su uso general** Esto es la continuación del servidor de archivos en clúster que se ha admitido en Windows Server desde la introducción de agrupación en clústeres de conmutación por error. Este tipo de servidor de archivos agrupado y, por lo tanto, todos los recursos compartidos asociados con el servidor de archivos agrupado, está en línea en un nodo a la vez. A veces se conoce como activa/pasiva o dual activo en ese momento. Recursos compartidos de archivos asociados con este tipo de servidor de archivos agrupado se denominan recursos compartidos de archivos agrupados en clúster. Éste es el tipo de servidor de archivos que se recomienda al implementar escenarios de trabajador de la información.

## <a name="scenario-description"></a>Descripción del escenario

Con recursos compartidos de archivos de escalado horizontal, puede compartir la misma carpeta de varios nodos de un clúster. Por ejemplo, si tiene un clúster de servidores de archivo de cuatro nodos que está usando el escalado de bloque de mensajes del servidor (SMB), un equipo que ejecute Windows Server 2012 R2 o Windows Server 2012 puede tener acceso a recursos compartidos de archivos desde cualquiera de los cuatro nodos. Esto se logra al aprovechar las nuevas características de clústeres de conmutación por error de Windows Server y las capacidades del protocolo de servidor de archivos de Windows, SMB 3.0. Los administradores de servidores de archivo pueden proporcionar los servicios de archivo continuamente disponibles para las aplicaciones de servidor y recursos compartidos de archivos de escalado horizontal y responder a las demandas de mayor rápidamente poniendo simplemente más servidores en línea. Todo esto puede realizarse en un entorno de producción y es completamente transparente para la aplicación de servidor.

Proporcionado por el servidor de archivos de escalado horizontal de los beneficios claves incluyen:

- **Recursos compartidos de archivos activa**. Todos los nodos del clúster pueden aceptar y atender las solicitudes de cliente SMB. Al hacer que el archivo compartir contenido sea accesible a través de todos los nodos del clúster de forma simultánea, clústeres de SMB 3.0 y los clientes cooperan para proporcionar conmutación por error transparente para los nodos de clúster alternativo durante el mantenimiento planificado y errores no planeados con servicio interrupción.
- **Mayor ancho de banda**. El ancho de banda máximo de recurso compartido es el ancho de banda total de todos los nodos del clúster de servidor de archivo. A diferencia de versiones anteriores de Windows Server, el ancho de banda total ya no está limitada al ancho de banda de un nodo de clúster único; pero, en su lugar, la capacidad de la copia de seguridad del sistema de almacenamiento define las restricciones. Puede aumentar el ancho de banda total mediante la adición de nodos.
- **CHKDSK con cero el tiempo de inactividad**. CHKDSK en Windows Server 2012 se ha mejorado considerablemente para reducir considerablemente el tiempo de que un sistema de archivos está sin conexión para reparar. Volúmenes compartidos agrupados en clúster (CSVs) dar un paso más al eliminar la fase sin conexión. Un sistema de archivo CSV (CSVFS) puede usar CHKDSK sin afectar a las aplicaciones con los identificadores abiertos en el sistema de archivos.
- **Memoria caché de volumen compartido de clúster**. CSVs en Windows Server 2012 presenta la compatibilidad con una memoria caché de lectura, que puede mejorar considerablemente el rendimiento en ciertos escenarios, como en la infraestructura de Escritorio Virtual (VDI).
- **Administración más sencilla**. Con el servidor de archivos de escalado horizontal, crear los servidores de archivos de escalado horizontal y, a continuación, agregue el CSVs necesarias y recursos compartidos de archivos. Ya no es necesario crear varios servidores de archivos en clúster, cada uno con los discos de clúster por separado y, a continuación, desarrollar las políticas de ubicación para asegurarse de actividad en cada nodo de clúster.
- **Automática reequilibrio de los clientes de servidor de archivos de escalado horizontal**. En Windows Server 2012 R2, reequilibrio automática mejora la escalabilidad y la capacidad de administración para servidores de archivos de escalado horizontal. Se realiza un seguimiento de las conexiones de cliente SMB por recurso compartido de archivos (en lugar de hacerlo por servidor), y los clientes, a continuación, se redirigen al nodo del clúster con el mejor acceso a del volumen utilizado por el recurso compartido de archivos. Esto mejora la eficiencia al reducir el tráfico de redirección entre los nodos de servidor de archivo. Los clientes se redirigen sigue una conexión inicial y cuando se vuelve a configurar el almacenamiento de clúster.

## <a name="in-this-scenario"></a>En este escenario

En los temas siguientes están disponibles para ayudarlo a implementar un servidor de archivos de escalado horizontal:

- [Plan para servidor de archivos de escalado horizontal](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/jj134258(v%3dws.11)>)

  - [Paso 1: Planeación de almacenamiento en el servidor de archivos de escalado horizontal](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/jj134181%28v%3dws.11%29>)
  - [Paso 2: Planeación de una red en el servidor de archivos de escalado horizontal](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/jj134253%28v%3dws.11%29>)

- [Implementar servidor de archivos de escalado horizontal](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/hh831359%28v%3dws.11%29>)

  - [Paso 1: Instalar los requisitos previos para el servidor de archivos de escalado horizontal](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/hh831478%28v%3dws.11%29>)
  - [Paso 2: Configurar servidor de archivos de escalado horizontal](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/hh831718%28v%3dws.11%29>)
  - [Paso 3: Configurar Hyper-V para usar el servidor de archivos de escalado horizontal](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/hh831463%28v%3dws.11%29>)
  - [Paso 4: Configurar Microsoft SQL Server para usar el servidor de archivos de escalado horizontal](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/hh831815%28v%3dws.11%29>)

## <a name="when-to-use-scale-out-file-server"></a>Cuándo se debe usar el servidor de archivos de escalado horizontal

Si la carga de trabajo genera una gran cantidad de operaciones de metadatos, como abrir archivos, no deben usar servidor de archivos de escalado horizontal cerrar archivos, crear nuevos archivos o cambio de nombre de los archivos existentes. Un trabajador de la información típica podría generar una gran cantidad de operaciones de metadatos. Debe usar un servidor de archivos de escalado horizontal, si está interesado en la escalabilidad y la simplicidad que ofrece y si sólo necesita tecnologías que son compatibles con el servidor de archivos de escalado horizontal.

En la siguiente tabla se enumera las capacidades de SMB 3.0, sistemas de archivos comunes de Windows, tecnologías de administración de datos de servidor de archivo y las cargas de trabajo comunes. Puede ver si la tecnología es compatible con el servidor de archivos de escalado horizontal, o si requiere un servidor de archivos en clúster tradicional (también conocido como un servidor de archivos para su uso general).

<table>
<thead>
<tr class="header">
<th>Área de tecnología</th>
<th>Función</th>
<th>Clúster de servidores de archivos de uso general</th>
<th>Servidor de archivos de escalado horizontal</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td>SMB</td>
<td>Disponibilidad continua de SMB</td>
<td>Sí</td>
<td>Sí</td>
</tr>
<tr class="even">
<td>SMB</td>
<td>Multicanal de SMB</td>
<td>Sí</td>
<td>Sí</td>
</tr>
<tr class="odd">
<td>SMB</td>
<td>Directo de SMB</td>
<td>Sí</td>
<td>Sí</td>
</tr>
<tr class="even">
<td>SMB</td>
<td>Cifrado de SMB</td>
<td>Sí</td>
<td>Sí</td>
</tr>
<tr class="odd">
<td>SMB</td>
<td>Conmutación por error transparente de SMB</td>
<td>Sí (si está habilitada la disponibilidad continua)</td>
<td>Sí</td>
</tr>
<tr class="even">
<td>Sistema de archivos</td>
<td>NTFS</td>
<td>Sí</td>
<td>NA</td>
</tr>
<tr class="odd">
<td>Sistema de archivos</td>
<td>Sistema de archivos resistente (<a href="https://docs.microsoft.com/windows-server/storage/refs/refs-overview">ReFS</a>)</td>
<td>Se recomienda con el almacenamiento directo de espacios</td>
<td>Se recomienda con el almacenamiento directo de espacios</td>
</tr>
<tr class="even">
<td>Sistema de archivos</td>
<td>Sistema de archivos por volumen (CSV) compartidos de clúster</td>
<td>NA</td>
<td>Sí</td>
</tr>
<tr class="odd">
<td>Administración de archivos</td>
<td>BranchCache</td>
<td>Sí</td>
<td>No</td>
</tr>
<tr class="even">
<td>Administración de archivos</td>
<td>Desduplicación de datos (Windows Server 2012)</td>
<td>Sí</td>
<td>No</td>
</tr>
<tr class="odd">
<td>Administración de archivos</td>
<td>Desduplicación de datos (Windows Server 2012 R2)</td>
<td>Sí</td>
<td>Sí (sólo VDI)</td>
</tr>
<tr class="even">
<td>Administración de archivos</td>
<td>Raíz del servidor DFS Namespace (DFSN) raíz</td>
<td>Sí</td>
<td>No</td>
</tr>
<tr class="odd">
<td>Administración de archivos</td>
<td>Servidor de destino de carpeta DFS Namespace (DFSN)</td>
<td>Sí</td>
<td>Sí</td>
</tr>
<tr class="even">
<td>Administración de archivos</td>
<td>Replicación DFS (DFSR)</td>
<td>Sí</td>
<td>No</td>
</tr>
<tr class="odd">
<td>Administración de archivos</td>
<td>File Server Resource Manager (pantallas y cuotas)</td>
<td>Sí</td>
<td>No</td>
</tr>
<tr class="even">
<td>Administración de archivos</td>
<td>Infraestructura de clasificación de archivo</td>
<td>Sí</td>
<td>No</td>
</tr>
<tr class="odd">
<td>Administración de archivos</td>
<td>Control de acceso dinámico (acceso basado en notificaciones, CAP)</td>
<td>Sí</td>
<td>No</td>
</tr>
<tr class="even">
<td>Administración de archivos</td>
<td>Redirección de carpetas</td>
<td>Sí</td>
<td>No se recomienda *</td>
</tr>
<tr class="odd">
<td>Administración de archivos</td>
<td>Archivos sin conexión (cliente el almacenamiento en caché del lado)</td>
<td>Sí</td>
<td>No se recomienda *</td>
</tr>
<tr class="even">
<td>Administración de archivos</td>
<td>Perfiles de usuario móvil</td>
<td>Sí</td>
<td>No se recomienda *</td>
</tr>
<tr class="odd">
<td>Administración de archivos</td>
<td>Directorios principales</td>
<td>Sí</td>
<td>No se recomienda *</td>
</tr>
<tr class="even">
<td>Administración de archivos</td>
<td>Carpetas de trabajo</td>
<td>Sí</td>
<td>No</td>
</tr>
<tr class="odd">
<td>NFS</td>
<td>Servidor NFS</td>
<td>Sí</td>
<td>No</td>
</tr>
<tr class="even">
<td>Aplicaciones</td>
<td>Hyper-V</td>
<td>No se recomienda</td>
<td>Sí</td>
</tr>
<tr class="odd">
<td>Aplicaciones</td>
<td>Microsoft SQL Server</td>
<td>No se recomienda</td>
<td>Sí</td>
</tr>
</tbody>
</table>

\ * Redirección de carpetas, archivos sin conexión, perfiles de usuario móvil o directorios principales generan un gran número de escrituras que deben escribirse en el disco (sin almacenamiento en búfer) inmediatamente al usar recursos compartidos de archivos disponibles en forma continua, reducción de rendimiento en comparación con recursos compartidos de archivos de propósito general. Continuamente los recursos compartidos de archivos disponibles también son incompatibles con File Server Resource Manager y equipos que ejecutan Windows XP. Además, los archivos sin conexión podría no realizar la transición al modo sin conexión para 3-6 minutos después de que un usuario pierde el acceso a un recurso compartido, que podría frustran a los usuarios que todavía no están usando el modo sin conexión siempre de archivos sin conexión.

## <a name="practical-applications"></a>Aplicaciones prácticas

Servidores de archivos de escalado horizontal son ideales para el almacenamiento de la aplicación de servidor. A continuación se muestran algunos ejemplos de aplicaciones de servidor que pueden almacenar sus datos en un recurso compartido de archivo de escalado horizontal:

- El servidor Web de Internet Information Services (IIS) puede almacenar configuración y los datos para los sitios Web en un recurso compartido de archivo de escalado horizontal. Para obtener más información, vea [Configuración compartida](http://www.iis.net/learn/manage/managing-your-configuration-settings/shared-configuration_264).
- Hyper-V puede almacenar la configuración y live discos virtuales en un recurso compartido de archivo de escalado horizontal. Para obtener más información, vea [Implementación de Hyper-V a través de SMB](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/jj134187(v%3dws.11)>).
- SQL Server puede almacenar los archivos de base de datos activa en un recurso compartido de archivo de escalado horizontal. Para obtener más información, vea [instalar SQL Server con el archivo SMB compartir como una opción de almacenamiento](https://docs.microsoft.com/sql/database-engine/install-windows/install-sql-server-with-smb-fileshare-as-a-storage-option).
- Virtual Machine Manager (VMM) puede almacenar un recurso compartido de biblioteca (que contiene plantillas de máquina virtual y los archivos relacionados) en un recurso compartido de archivo de escalado horizontal. Sin embargo, el propio servidor de biblioteca no puede ser un servidor de archivos de escalado horizontal: debe estar en un servidor independiente o en un clúster de conmutación por error que no use la función de clúster de servidor de archivos de escalado horizontal.

Si usa un recurso compartido de archivo de escalado horizontal como un recurso compartido de biblioteca, puede usar sólo las tecnologías que son compatibles con el servidor de archivos de escalado horizontal. Por ejemplo, no puede usar la replicación DFS para replicar un recurso compartido de biblioteca hospedado en un recurso compartido de archivo de escalado horizontal. También es importante que el servidor de archivos de escalado horizontal que tenga instaladas las actualizaciones de software más recientes.

Para usar un recurso compartido de archivo de escalado horizontal como un recurso compartido de biblioteca, en primer lugar agregar un servidor de biblioteca (es probable que una máquina virtual) con un recurso compartido local o no hay recursos compartidos en absoluto. A continuación, cuando se agrega un recurso compartido de biblioteca, elija un recurso compartido de archivos que se hospeda en un servidor de archivos de escalado horizontal. Este recurso compartido debe ser administrada con VMM y creado exclusivamente para su uso por el servidor de biblioteca. Además, asegúrese de instalar las actualizaciones más recientes en el servidor de archivos de escalado horizontal. Para obtener más información acerca de cómo agregar servidores de biblioteca VMM y recursos compartidos de biblioteca, vea [Agregar perfiles a la biblioteca de VMM](https://docs.microsoft.com/system-center/vmm/library-profiles?view=sc-vmm-1801). Para obtener una lista de revisiones actualmente disponibles para servicios de almacenamiento y de archivo, vea [el artículo de Microsoft Knowledge Base 2899011](https://support.microsoft.com/help/2899011/list-of-currently-available-hotfixes-for-the-file-services-technologie).

>[!NOTE]
>Algunos usuarios, como los trabajadores de información, tienen las cargas de trabajo que tienen un impacto mayor en el rendimiento. Por ejemplo, las operaciones, como abrir y cerrar archivos, crear nuevos archivos y cambiar el nombre de los archivos existentes, cuando se realizan por varios usuarios, tienen un impacto en el rendimiento. Si un recurso compartido de archivos está habilitado con disponibilidad continua, proporciona integridad de los datos, pero también afecta al rendimiento general. Disponibilidad continua requiere que escriben datos a través de en el disco para garantizar la integridad en caso de error de un nodo de clúster en un servidor de archivos de escalado horizontal. Por lo tanto, un usuario que copia varios archivos de gran tamaño en un servidor de archivos puede esperar un rendimiento considerablemente más lento en recurso compartido de archivos disponibles en forma continua.

## <a name="features-included-in-this-scenario"></a>Características que se incluyen en este escenario

En la siguiente tabla se enumera las características que forman parte de este escenario y se describe cómo apoyan.

<table>
<thead>
<tr class="header">
<th>Función</th>
<th>¿Cómo es compatible con este escenario</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><a href="failover-clustering.md">Clúster de conmutación por error</a></td>
<td>Clústeres de conmutación por error agregan las siguientes características en Windows Server 2012 para admitir el servidor de archivos de escalado horizontal: nombre de red distribuida, el tipo de recurso de servidor de archivos de escalado horizontal, volúmenes compartidos clúster (CSV) 2 y el rol de escalado horizontal archivo alta disponibilidad del servidor. Para obtener más información acerca de estas características, consulte <a href="https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/dn265972(v%3dws.11)">Novedades de agrupación en clústeres de conmutación por error en Windows Server 2012 [redirigen]</a>.</td>
</tr>
<tr class="even">
<td><a href="https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/hh831795(v%3dws.11)">Bloque de mensajes del servidor</a></td>
<td>SMB 3.0 agrega las siguientes características de Windows Server 2012 para admitir el escalado horizontal servidor de archivos: conmutación por error transparente de SMB, multicanal de SMB y directo de SMB.<br />
<br />
Para obtener más información sobre las funciones nuevas y modificadas de SMB en Windows Server 2012 R2, consulte <a href="https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/hh831474(v%3dws.11)">What ' s New in SMB en Windows Server</a>.</td>
</tr>
</tbody>
</table>

## <a name="more-information"></a>Más información

- [Guía de consideraciones de diseño de almacenamiento de información definidas por el software](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/mt243829(v%3dws.11)>)
- [El aumento de servidor, el almacenamiento y la disponibilidad de la red](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/hh831437(v%3dws.11)>)
- [Implementar Hyper-V en SMB](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/jj134187(v%3dws.11)>)
- [Implementación de servidores de archivos rápida y eficaz para aplicaciones de servidor](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/hh831723(v%3dws.11)>)
- [Para cambiar la escala out o no a escalado horizontal, es la pregunta](https://blogs.technet.com/b/filecab/archive/2013/12/05/to-scale-out-or-not-to-scale-out-that-is-the-question.aspx) (entrada de blog)
- [Redireccionamiento de carpetas, archivos sin conexión y perfiles de usuario móvil](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/hh848267(v%3dws.11)>)