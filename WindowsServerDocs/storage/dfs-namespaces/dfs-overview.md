---
title: "Información general de Espacios de nombres DFS"
ms.prod: windows-server-threshold
ms.author: jgerend
ms.manager: brianlic
ms.technology: storage
ms.topic: article
author: jasongerend
ms.date: 07/10/2017
description: "En este tema se describen los espacios de nombres DFS, que es un servicio de rol de Windows Server que te permite agrupar las carpetas compartidas ubicadas en distintos servidores en uno o varios espacios de nombres estructurados lógicamente."
ms.openlocfilehash: f3a76208fa1d6e1207edd699f830fc05a360bee5
ms.sourcegitcommit: 583355400f6b0d880dc0ac6bc06f0efb50d674f7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/17/2017
---
# <a name="dfs-namespaces-overview"></a>Información general de Espacios de nombres DFS

> Se aplica a: Windows Server (canal semianual), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2, Windows Server 2008

Espacios de nombres DFS es un servicio de rol de Windows Server que te permite agrupar las carpetas compartidas ubicadas en distintos servidores en uno o varios espacios de nombres estructurados lógicamente. Esto permite ofrecer a los usuarios una vista virtual de las carpetas compartidas, donde una única ruta de acceso conduce a los archivos ubicados en varios servidores, tal y como se muestra en la siguiente figura:

![Elementos de la tecnología de Espacios de nombres DFS](media/dfs-overview.png)

Aquí se muestra una descripción de los elementos que componen un espacio de nombres DFS:

- **Servidor de espacio de nombres**: un servidor de espacio de nombres que aloja un espacio de nombre. El servidor de espacio de nombres puede ser un servidor miembro o un controlador de dominio.
- **Raíz de espacio de nombres**: la raíz de espacio de nombres es el punto inicial del espacio de nombres. En la figura anterior, el nombre de la raíz es público (Public) y la ruta de acceso del espacio de nombres es \\\Contoso\\Public. Este tipo de espacio de nombres es un espacio de nombres basado en dominio, ya que comienza con un nombre de dominio (por ejemplo, Contoso) y sus metadatos se almacenan en los Servicios de dominio de Active Directory (AD DS). Aunque en la figura anterior se muestra un único servidor de espacio de nombres, un espacio de nombres basado en dominio puede alojarse en varios servidores de espacio de nombres para aumentar la disponibilidad del espacio de nombres.
- **Carpeta**: las carpetas sin destinos de carpeta agregan estructura y jerarquía al espacio de nombres y las carpetas con destinos de carpeta ofrecen a los usuarios contenido real. Cuando los usuarios exploran una carpeta que incluye los destinos de carpeta en el espacio de nombres, el equipo cliente recibe una referencia que redirige de forma transparente al equipo cliente a uno de los destinos de carpeta.
- **Destinos de carpeta**: un destino de carpeta es la ruta de acceso de UNC de una carpeta compartida u otro espacio de nombres que está asociado a una carpeta de un espacio de nombres. El destino de carpeta es donde se almacenan los datos y el contenido. En la figura anterior, la carpeta denominada Herramientas (Tools) tiene dos destinos de carpeta, uno en Londres (London) y otro en Nueva York (New York), y la carpeta denominada Guías de aprendizaje (Training Guides) tiene un sola carpeta de destino en Nueva York (New York). Un usuario que se desplaza a \\\Contoso\\Public\\Software\\Tools se redirige de forma transparente a la carpeta compartida \\\LDN-SVR-01\\Tools o \\\NYC-SVR-01\\Tools, en función del lugar en el que se encuentre actualmente el usuario.

En este tema se describe la instalación de DFS, las novedades y dónde encontrar información de evaluación e implementación.

Puedes administrar espacios de nombres mediante la opción Administración de DFS, los [Cmdlets de espacios de nombres DFS (DFSN) en WindowsPowerShell](https://technet.microsoft.com/library/jj884270.aspx), el comando **DfsUtil** o scripts que llaman a WMI.

## <a name="server-requirements-and-limits"></a>Límites y requisitos del servidor

No hay requisitos de hardware ni software adicionales para ejecutar Administración de DFS o usar los espacios de nombres DFS.

Un servidor de espacio de nombres es un controlador de dominio o un servidor miembro que aloja un espacio de nombres. El sistema operativo que se ejecuta en el servidor de espacio de nombres determina el número de espacios de nombres que puedes alojar en un servidor.

Los servidores que ejecutan los siguientes sistemas operativos pueden alojar varios espacios de nombres basados en dominios y un único espacio de nombres independiente. 

- Windows Server (canal semianual)
- WindowsServer2016 
- Windows Server2012R2
- Windows Server 2012
- Windows Server 2008 R2 Datacenter/Enterprise

Los servidores que ejecutan los siguientes sistemas operativos pueden alojar un único espacio de nombres independiente:

- Windows Server 2008 R2 Standard


En la siguiente tabla se describen factores adicionales a tener en cuenta al elegir servidores para alojar un espacio de nombres.

|Servidor que aloja espacios de nombres independientes|Servidor que aloja espacios de nombres basados en dominios|
|---|---|
|Debe incluir un volumen NTFS para alojar el espacio de nombres.|Debe incluir un volumen NTFS para alojar el espacio de nombres.|
|Puede ser un servidor miembro o un controlador de dominio.|Debe ser un servidor miembro o un controlador de dominio en el dominio en el que se ha configurado el espacio de nombres. (Este requisito se aplica a cada servidor de espacio de nombres que aloja un espacio de nombres basado en un dominio determinado).|
|Puede estar alojado por un clúster de conmutación por error para aumentar la disponibilidad del espacio de nombres.|El espacio de nombres no puede ser un recurso en clúster en un clúster de conmutación por error. Pero puedes ubicar el espacio de nombres en un servidor que también funcione como un nodo en un clúster de conmutación por error si configuras el espacio de nombres para usar solo los recursos locales en dicho servidor.|

## <a name="installing-dfs-namespaces"></a>Instalar Espacios de nombres DFS

Espacios de nombres DFS y Replicación DFS forman parte del rol Servicios de archivos y almacenamiento. Las herramientas de administración de DFS (Administración de DFS, el módulo Espacios de nombres DFS para WindowsPowerShell y las herramientas de línea de comandos) se instalan por separado como parte de las Herramientas de administración remota del servidor.

Instala Espacios de nombres DFS mediante el Administrador del servidor o PowerShell, tal y como se describe en las secciones siguientes.

### <a name="to-install-dfs-by-using-server-manager"></a>Para instalar DFS mediante el Administrador del servidor

1. Abre el Administrador del servidor, haz clic en **Administrar**y luego haz clic en **Agregar roles y características**. Aparece el Asistente para agregar roles y características.

2. En la página **Selección de servidor**, selecciona el servidor o el disco duro virtual (VHD) de una máquina virtual sin conexión en la que quieras instalar DFS.

3. Selecciona los servicios de rol y las características que quieras instalar.

    - Para instalar el servicio de espacios de nombres DFS, en la página **Roles de servidor**, selecciona **Espacios de nombres DFS**.

    - Para instalar únicamente las Herramientas de administración de DFS, en la página **Características**, expande **Herramientas de administración remota del servidor**, **Herramientas de administración de roles**, expande **Herramientas de Servicios de archivo** y luego selecciona **Herramientas de administración de DFS**.

         **Herramientas de administración de DFS** instala el complemento Administración de DFS, el módulo Espacios de nombres DFS para WindowsPowerShell y las herramientas de línea de comandos, pero no instala ningún servicio DFS en el servidor.

### <a name="to-install-dfs-by-using-windows-powershell"></a>Para instalar DFS mediante WindowsPowerShell

Abre una sesión de WindowsPowerShell con permisos de usuario elevados y luego escribe el siguiente comando, donde <name\> es el servicio de rol o la características que deseas instalar (consulta la siguiente tabla para ver una lista de los nombres de servicios de rol o características relevantes):

```PowerShell
Install-WindowsFeature <name>
```

|Servicio de rol o característica|Nombre|
|---|---|
|Espacios de nombres DFS|`FS-DFS-Namespace`|
|Herramientas de administración de DFS|`RSAT-DFS-Mgmt-Con`|

Por ejemplo, para instalar la parte de Herramientas del sistema de archivos distribuido de la característica Herramientas de administración remota del servidor, escribe:

```PowerShell
Install-WindowsFeature RSAT-DFS-Mgmt-Con
```

Para instalar las partes de Espacios de nombres DFS y Herramientas del sistema de archivos distribuido de la característica Herramientas de administración remota del servidor, escribe:

```PowerShell
Install-WindowsFeature FS-DFS-Namespace, RSAT-DFS-Mgmt-Con
```

## <a name="interoperability-with-azure-virtual-machines"></a>Interoperabilidad con máquinas virtuales de Azure

Se ha probado el uso de Espacios de nombres DFS en una máquina virtual de Microsoft Azure, pero hay algunas limitaciones y requisitos que se deben seguir.

- No se pueden agrupar en clústeres los espacios de nombres independientes en máquinas virtuales de Azure.

- Puedes alojar espacios de nombres basados en dominios en máquinas virtuales de Azure, incluidos los entornos con Azure Active Directory, aunque un único espacio de nombres no puede incluir servidores de espacio de nombres locales y servidores de espacio de nombres alojados en máquinas virtuales de Azure, incluso cuando se usa Servicios de federación de Active Directory (AD FS).

Para obtener más información sobre cómo comenzar a usar las máquinas virtuales de Azure, consulta la [Documentación de máquinas virtuales de Azure](https://docs.microsoft.com/azure/virtual-machines/).

## <a name="see-also"></a>Consulta también

Para obtener más información relacionada, consulta los siguientes recursos.

|Tipo de contenido|Referencias|
|------------------|----------------|
|**Evaluación del producto**|[Novedades de Espacios de nombres DFS y Replicación DFS en WindowsServer](https://technet.microsoft.com/library/dn281957(v=ws.11).aspx)|
|**Implementación**|[Consideraciones sobre la escalabilidad de Espacios de nombres DFS](http://blogs.technet.com/b/filecab/archive/2012/08/26/dfs-namespace-scalability-considerations.aspx)|
|**Operaciones**|[Espacios de nombres DFS: preguntas más frecuentes](http://technet.microsoft.com/library/ee404780.aspx)|
|**Recursos de la comunidad**|[Foro de TechNet sobre el almacenamiento y los servicios de archivos](http://social.technet.microsoft.com/forums/winserverfiles/threads/)|
|**Protocolos**|[Protocolos de WindowsServer de Servicios de archivos](http://msdn.microsoft.com/library/cc239875.aspx)|
|**Tecnologías relacionadas**| [Clúster de conmutación por error](../../failover-clustering/failover-clustering-overview.md)|