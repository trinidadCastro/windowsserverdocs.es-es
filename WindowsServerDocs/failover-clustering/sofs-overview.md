---
title: Introducción al servidor de archivos de escalabilidad horizontal para datos de aplicación
description: Información general de la característica Servidor de archivos de escalabilidad horizontal para Windows Server 201 R2 y Windows Server 2012.
ms.topic: article
author: JasonGerend
ms.author: jgerend
manager: lizross
ms.date: 04/26/2018
ms.localizationpriority: medium
ms.openlocfilehash: 9147d88b2d31def46e7a755abf17a2cdf767f8a2
ms.sourcegitcommit: 68444968565667f86ee0586ed4c43da4ab24aaed
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/07/2020
ms.locfileid: "87990691"
---
# <a name="scale-out-file-server-for-application-data-overview"></a>Introducción al servidor de archivos de escalabilidad horizontal para datos de aplicación

>Se aplica a: Windows Server 2012 R2, Windows Server 2012

El Servidor de archivos de escalabilidad horizontal es una característica que se diseñó para proporcionar recursos compartidos de archivos de escalabilidad horizontal que están disponibles continuamente para el almacenamiento de aplicaciones del servidor basado en archivos. Los recursos compartidos de archivos de escalabilidad horizontal otorgan la capacidad de compartir la misma carpeta desde varios nodos del mismo clúster. Este escenario se centra en cómo planear e implementar el Servidor de archivos de escalabilidad horizontal.

Puedes implementar y configurar un servidor de archivos en clúster usando los métodos siguientes:

- **Servidor de archivos de escalabilidad horizontal para los datos de la aplicación** Esta característica de servidor de archivos en clúster se presentó en Windows Server 2012 y permite almacenar datos de aplicaciones de servidor, como archivos de máquina virtual de Hyper-V, en recursos compartidos de archivos y obtener un nivel similar de confiabilidad, disponibilidad, facilidad de uso y alto rendimiento que se esperaría de una red de área de almacenamiento. Todos los recursos compartidos de archivos se encuentran en línea en todos los nodos de forma simultánea. Los recursos compartidos de archivos asociados con este tipo de servidor de archivos en clúster se denominan recursos compartidos de archivos de escalabilidad horizontal. A menudo, esto se denomina activo-activo. Este es el tipo de servidor de archivos recomendado al implementar tanto Hyper-V en el Bloque de mensajes del servidor (SMB) como Microsoft SQL Server en SMB.
- **Servidor de archivos para uso general** Esta es la continuación del servidor de archivos en clúster que se admite en Windows Server desde la introducción de los clústeres de conmutación por error. Este tipo de servidor de archivos en clúster (y, por lo tanto, todos los recursos compartidos asociados con el servidor de archivos en clúster) se encuentra en línea en un nodo a la vez. Algunas veces, esto se denomina activo-pasivo o doble-activo. Los recursos compartidos de archivos asociados con este tipo de servidor de archivos en clúster se denominan recursos compartidos de archivos en clúster. Este es el tipo de servidor de archivos recomendado al implementar escenarios de Trabajador de la información.

## <a name="scenario-description"></a>Descripción del escenario

Con los recursos compartidos de archivos de escalabilidad horizontal puedes compartir la misma carpeta desde varios nodos de un clúster. Por ejemplo, si tiene un clúster de servidores de archivos de cuatro nodos que usa el escalado horizontal del bloque de mensajes del servidor (SMB), un equipo que ejecute Windows Server 2012 R2 o Windows Server 2012 podrá tener acceso a recursos compartidos de archivos desde cualquiera de los cuatro nodos. Esto se logra al aprovechar las nuevas características del clúster de conmutación por error de Windows Server y las capacidades del protocolo del servidor de archivos de Windows, SMB 3.0. Los administradores del servidor de archivos pueden proporcionar recursos compartidos de archivos de escalabilidad horizontal y servicios de archivos disponibles continuamente a las aplicaciones del servidor y responder a una mayor demanda con rapidez al colocar más servidores en línea. Todo esto se puede realizar en un entorno de producción y es completamente transparente a la aplicación del servidor.

Entre los beneficios clave que proporciona el servidor de archivos de escalabilidad horizontal se incluyen:

- **Recursos compartidos de archivos activos-activos**. Todos los nodos de clúster pueden aceptar y atender solicitudes de cliente SMB. Al permitir que todo el contenido del recurso compartido de archivos esté accesible a través de todos los nodos de clúster simultáneamente, los clientes y los clústeres de SMB 3.0 cooperan para proporcionar una conmutación por error transparente a los nodos de clúster alternativos durante el mantenimiento planeado o errores no planeados con interrupción del servicio.
- **Mayor ancho de banda**. El ancho de banda máximo del recurso compartido es el ancho de banda total de todos los nodos del clúster de servidores de archivos. A diferencia de las versiones anteriores de Windows Server, el ancho de banda total ya no se ve restringido por el ancho de banda de un solo nodo de clúster, sino que la capacidad del sistema de almacenamiento de respaldo define las restricciones. Puede aumentar el ancho de banda total al agregar nodos.
- **CHKDSK sin tiempo de inactividad**. CHKDSK en Windows Server 2012 se ha mejorado significativamente para acortar drásticamente el tiempo que un sistema de archivos está sin conexión para la reparación. Los volúmenes compartidos en clúster (CSV) están un paso adelante ya que eliminan la fase de dejar sin conexión. Un sistema de archivos CSV (CSVFS) puede usar CHKDSK sin afectar a las aplicaciones con identificadores abiertos en el sistema de archivos.
- **Caché de volumen compartido en clúster**. CSV en Windows Server 2012 incorpora compatibilidad para una caché de lectura, lo que puede mejorar significativamente el rendimiento en ciertos escenarios, como en la infraestructura de escritorio virtual (VDI).
- **Administración más sencilla**. Con Servidor de archivos de escalabilidad horizontal, puede crear los servidores de archivos de escalabilidad horizontal y, a continuación, agregar los recursos compartidos de archivos y CSV necesarios. Ya no es necesario crear varios servidores de archivos en clúster, cada uno con discos de clúster separados y después desarrollar directivas de colocación para garantizar la actividad en cada nodo de clúster.
- **Reequilibrio automático de clientes de servidor de archivos de escalabilidad horizontal**. En Windows Server 2012 R2, el reequilibrio automático mejora la escalabilidad y la capacidad de administración de los servidores de archivos de escalabilidad horizontal. El seguimiento de las conexiones de cliente SMB se realiza por recurso compartido de archivos (en lugar de por servidor) y, a continuación, se redirige a los clientes al nodo del clúster con el mejor acceso al volumen usado por el recurso compartido de archivos. Esto mejora la eficiencia al reducir el tráfico de redirección entre los nodos del servidor de archivos. Los clientes se redirigen tras una conexión inicial y cuando se vuelve a configurar el almacenamiento de clúster.

## <a name="in-this-scenario"></a>En este escenario

Los temas siguientes están disponibles para ayudarte a implementar un servidor de archivos de escalabilidad horizontal:

- [Plan para servidor de archivos de escalabilidad horizontal](</previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/jj134258(v%3dws.11)>)

  - [Paso 1: Plan para almacenamiento en servidor de archivos de escalabilidad horizontal](</previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/jj134181%28v%3dws.11%29>)
  - [Step 2: Plan for Networking in Scale-Out File Server](</previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/jj134253%28v%3dws.11%29>)

- [Deploy Scale-Out File Server](</previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/hh831359%28v%3dws.11%29>)

  - [Paso 1: Instalar requisitos previos para el servidor de archivos de escalabilidad horizontal](</previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/hh831478%28v%3dws.11%29>)
  - [Paso 2: Configurar el servidor de archivos de escalabilidad horizontal](</previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/hh831718%28v%3dws.11%29>)
  - [Paso 3: Configurar Hyper-V para usar el servidor de archivos de escalabilidad horizontal](</previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/hh831463%28v%3dws.11%29>)
  - [Paso 4: Configurar Microsoft SQL Server para usar el servidor de archivos de escalabilidad horizontal](</previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/hh831815%28v%3dws.11%29>)

## <a name="when-to-use-scale-out-file-server"></a>Cuándo utilizar el servidor de archivos de escalabilidad horizontal

No debe utilizar el servidor de archivos de escalabilidad horizontal si la carga de trabajo genera un alto número de operaciones de metadatos, como abrir archivos, cerrar archivos, crear archivos nuevos o cambiar el nombre de archivos existentes. Un trabajador de la información típico generaría muchas operaciones de metadatos. Debes utilizar un servidor de archivos de escalabilidad horizontal si estás interesado en la escalabilidad y la simplicidad que ofrece y si solo necesitas tecnologías que sean compatibles con el servidor de archivos de escalabilidad horizontal.

En la tabla siguiente se enumeran capacidades de SMB 3.0, sistemas de archivos comunes de Windows, tecnologías de administración de datos de servidor de archivos y cargas de trabajo comunes. Puedes comprobar si la tecnología es compatible con el servidor de archivos de escalabilidad horizontal, o si requiere un servidor de archivos en clúster tradicional (también conocido como un servidor de archivos para uso general).

<table>
<thead>
<tr class="header">
<th>Área de tecnología</th>
<th>Característica</th>
<th>Clúster de servidor de archivos de uso general</th>
<th>Servidor de archivos de escalabilidad horizontal</th>
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
<td>SMB multicanal</td>
<td>Sí</td>
<td>Sí</td>
</tr>
<tr class="odd">
<td>SMB</td>
<td>SMB directo</td>
<td>Sí</td>
<td>Sí</td>
</tr>
<tr class="even">
<td>SMB</td>
<td>Cifrado SMB</td>
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
<td>N/D</td>
</tr>
<tr class="odd">
<td>Sistema de archivos</td>
<td>Sistema de archivos resistente (<a href="https://docs.microsoft.com/windows-server/storage/refs/refs-overview">ReFS</a>)</td>
<td>Recomendado con Espacios de almacenamiento directo</td>
<td>Recomendado con Espacios de almacenamiento directo</td>
</tr>
<tr class="even">
<td>Sistema de archivos</td>
<td>Sistema de archivos de Volúmen compartido de clúster (CSV)</td>
<td>N/D</td>
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
<td>Sí (solo VDI)</td>
</tr>
<tr class="even">
<td>Administración de archivos</td>
<td>Servidor de raíz de la raíz de Espacio de nombres DFS (DFSN)</td>
<td>Sí</td>
<td>No</td>
</tr>
<tr class="odd">
<td>Administración de archivos</td>
<td>Servidor de destino de la carpeta de Espacio de nombres DFS (DFSN)</td>
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
<td>Administrador de recursos del servidor de archivos (pantallas y cuotas)</td>
<td>Sí</td>
<td>No</td>
</tr>
<tr class="even">
<td>Administración de archivos</td>
<td>Infraestructura de clasificación de archivos</td>
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
<td>No recomendado<em></td>
</tr>
<tr class="odd">
<td>Administración de archivos</td>
<td>Archivos sin conexión (caché del lado cliente)</td>
<td>Sí</td>
<td>No recomendado</em></td>
</tr>
<tr class="even">
<td>Administración de archivos</td>
<td>Perfiles de usuario móviles</td>
<td>Sí</td>
<td>No recomendado<em></td>
</tr>
<tr class="odd">
<td>Administración de archivos</td>
<td>Directorios principales</td>
<td>Sí</td>
<td>No recomendado</em></td>
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
<td>No recomendado</td>
<td>Sí</td>
</tr>
<tr class="odd">
<td>Aplicaciones</td>
<td>Microsoft SQL Server</td>
<td>No recomendado</td>
<td>Sí</td>
</tr>
</tbody>
</table>

\*La redirección de carpetas, Archivos sin conexión, perfiles de usuario móviles o directorios principales generan un gran número de escrituras que se deben escribir inmediatamente en el disco (sin almacenamiento en búfer) al usar recursos compartidos de archivos disponibles continuamente, lo que reduce el rendimiento en comparación con los recursos compartidos de archivos de uso general. Los recursos compartidos de archivos disponibles continuamente también son incompatibles con el Administrador de recursos del servidor de archivos y con los equipos que ejecutan Windows XP. Además, es posible que Archivos sin conexión no pase al modo sin conexión durante 3-6 minutos después de que un usuario pierda el acceso a un recurso compartido, lo que podría frustrar a los usuarios que todavía no utilicen el modo sin conexión de Archivos sin conexión.

## <a name="practical-applications"></a>Aplicaciones prácticas

Los Servidores de archivos de escalabilidad horizontal son ideales para el almacenamiento de aplicaciones de servidor. A continuación se muestran algunos ejemplos de aplicaciones de servidor que pueden almacenar sus datos en un recurso compartido de archivos de escalabilidad horizontal:

- El servidor web de Internet Information Services (IIS) puede almacenar datos y la configuración de sitios web en un recurso compartido de archivos de escalabilidad horizontal. Para obtener más información, consulta [Configuración compartida](https://www.iis.net/learn/manage/managing-your-configuration-settings/shared-configuration_264).
- Hyper-V puede almacenar la configuración y los discos virtuales activos en un recurso compartido de archivos de escalabilidad horizontal. Para obtener más información, consulte [Implementación de Hyper-V en SMB](</previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/jj134187(v%3dws.11)>).
- SQL Server puede almacenar archivos de bases de datos activos en un recurso compartido de archivos de escalabilidad horizontal. Para obtener más información, consulta [Instalar SQL Server con recurso compartido de archivos SMB como una opción de almacenamiento](/sql/database-engine/install-windows/install-sql-server-with-smb-fileshare-as-a-storage-option).
- El Virtual Machine Manager (VMM) puede almacenar un recurso compartido de biblioteca (que contiene plantillas de máquinas virtuales y archivos relacionados) en un recurso compartido de archivos de escalabilidad horizontal. Sin embargo, el propio servidor de biblioteca no puede ser un Servidor de archivos de escalabilidad horizontal; debe estar en un servidor independiente o en un clúster de conmutación por error que no utilice el rol de clúster de Servidor de archivos de escalabilidad horizontal.

Si usas un recurso compartido de archivos de escalabilidad horizontal como un recurso compartido de biblioteca, puedes usar solamente tecnologías que sean compatibles con el servidor de archivos de escalabilidad horizontal. Por ejemplo, no puede usar Replicación DFS para replicar un recurso compartido de biblioteca hospedado en un recurso compartido de archivos de escalabilidad horizontal. También es importante que el servidor de archivos de escalabilidad horizontal tenga instaladas las actualizaciones de software más recientes.

Para usar un recurso compartido de archivos de escalabilidad horizontal como un recurso compartido de biblioteca, primero agrega un servidor de biblioteca (probablemente una máquina virtual) con un recurso compartido local o sin ningún recurso compartido. A continuación, cuando agregue un recurso compartido de biblioteca, elija un recurso compartido de archivos que se hospede en un servidor de archivos de escalabilidad horizontal. Este recurso compartido tiene que estar administrado por VMM y que se haya creado exclusivamente para su uso por el servidor de biblioteca. Además, asegúrate de instalar las actualizaciones más recientes en el servidor de archivos de escalabilidad horizontal. Para obtener más información acerca de cómo agregar servidores de biblioteca VMM y recursos compartidos de biblioteca, consulte [adición de perfiles a la biblioteca VMM](/system-center/vmm/library-profiles?view=sc-vmm-1801). Para obtener una lista de las revisiones disponibles actualmente para los Servicios de archivos y almacenamiento, consulta [el artículo de Microsoft Knowledge Base 2899011](https://support.microsoft.com/help/2899011/list-of-currently-available-hotfixes-for-the-file-services-technologie).

>[!NOTE]
>Algunos usuarios, como por ejemplo los trabajadores de la información, tienen cargas de trabajo que tienen un mayor impacto en el rendimiento. Por ejemplo, las operaciones como abrir y cerrar archivos, crear archivos nuevos y cambiar el nombre de los archivos existentes, cuando lo realizan varios usuarios, afecta al rendimiento. Si un recurso compartido de archivos está habilitado con disponibilidad continua, proporciona integridad de datos, pero también afecta al rendimiento general. La disponibilidad continua requiere que los datos escriban a través del disco para garantizar la integridad si se produce un error en un nodo de clúster de un servidor de archivos de escalabilidad horizontal. Por lo tanto, un usuario que copia varios archivos grandes en un servidor de archivos, puede esperar un rendimiento considerablemente más lento en el recurso compartido de archivos disponibles continuamente.

## <a name="features-included-in-this-scenario"></a>Características incluidas en este escenario

En la tabla siguiente, se enumeran las características que forman parte de este escenario y se describe la manera en que son compatibles con él.

<table>
<thead>
<tr class="header">
<th>Característica</th>
<th>Compatibilidad con este escenario</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><a href="failover-clustering.md">Clústeres de conmutación por error</a></td>
<td>Los clústeres de conmutación por error agregaron las siguientes características en Windows Server 2012 para admitir el servidor de archivos de escalabilidad horizontal: nombre de red distribuida, el tipo de recurso Servidor de archivos de escalabilidad horizontal, volúmenes compartidos de clúster (CSV) 2 y el rol de alta disponibilidad Servidor de archivos de escalabilidad horizontal. Para obtener más información acerca de estas características, vea <a href="/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/dn265972(v%3dws.11)">qué&#39;s novedades en los clústeres de conmutación por error en Windows Server 2012 [Redirigido]</a>.</td>
</tr>
<tr class="even">
<td><a href="/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/hh831795(v%3dws.11)">Bloque de mensajes del servidor</a></td>
<td>SMB 3,0 agregó las siguientes características en Windows Server 2012 para admitir el servidor de archivos de escalabilidad horizontal: conmutación por error transparente de SMB, SMB multicanal y SMB directo.<br />
<br />
Para obtener más información sobre las funciones nuevas y modificadas para SMB en Windows Server 2012 R2, consulte <a href="/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/hh831474(v%3dws.11)">¿qué&#39;es nuevo en SMB en Windows Server</a>?.</td>
</tr>
</tbody>
</table>

## <a name="more-information"></a>Más información

- [Guía de consideraciones de diseño de almacenamiento definidas mediante software](</previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/mt243829(v%3dws.11)>)
- [Increasing Server, Storage, and Network Availability](</previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/hh831437(v%3dws.11)>)
- [Implementación de Hyper-V en SMB](</previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/jj134187(v%3dws.11)>)
- [Implementación de servidores de archivos rápidos y eficaces para aplicaciones de servidores](</previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/hh831723(v%3dws.11)>)
- Entrada de blog [Escalar horizontalmente o no escalar horizontalmente, esa es la cuestión](https://blogs.technet.com/b/filecab/archive/2013/12/05/to-scale-out-or-not-to-scale-out-that-is-the-question.aspx)
- [Redireccionamiento de carpetas, Archivos sin conexión y Perfiles de usuario móvil](</previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/hh848267(v%3dws.11)>)