---
title: Introducción a la replicación DFS
ms.date: 03/08/2019
author: JasonGerend
manager: elizapo
ms.author: jgerend
ms.openlocfilehash: 6a68d27ec7c1d06e070d18362de68e12ecbf9578
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/07/2020
ms.locfileid: "87950759"
---
# <a name="dfs-replication-overview"></a>Introducción a la replicación DFS

> Se aplica a: Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2, Windows Server 2008, Windows Server (canal semianual)

La replicación DFS es un servicio de rol en Windows Server que permite replicar carpetas de manera eficaz (incluidas aquellas a las que hace referencia la ruta de acceso de un espacio de nombres DFS) en varios servidores y sitios. la replicación DFS es un motor de replicación con varios maestros eficaz que sirve para mantener las carpetas sincronizadas entre los servidores en las conexiones de red con ancho de banda limitado. Reemplaza el servicio de replicación de archivos (FRS) como motor de replicación para los espacios de nombres DFS, así como para replicar la carpeta SYSVOL de Active Directory Domain Services (AD DS) en dominios que usan el nivel funcional de dominio de Windows Server 2008 o posterior.

Replicación DFS usa un algoritmo de compresión denominado compresión diferencial remota (RDC). RDC detecta los cambios en los datos de un archivo y permite que la replicación DFS replique sólo los bloques de archivo modificados en lugar del archivo completo.

Para obtener más información acerca de la replicación de SYSVOL mediante replicación DFS, consulta [Migración de la replicación de SYSVOL a replicación DFS](migrate-sysvol-to-dfsr.md).

Para usar la replicación DFS, debes crear grupos de replicación y agregar carpetas replicadas a los grupos. Los grupos de replicación, las carpetas replicadas y los miembros se ilustran en la siguiente figura.

![Un grupo de replicación que contiene una conexión entre dos miembros, cada uno con un par de carpetas replicadas](media/dfsr-overview.gif)

Esta figura muestra que un grupo de replicación es un conjunto de servidores, llamados miembros, que participan en la replicación de una o más carpetas replicadas. Una carpeta replicada es una carpeta que se mantiene sincronizada en cada uno de los miembros. En la figura, hay dos carpetas replicadas: Proyectos y Propuestas. A medida que cambian los datos en cada carpeta replicada, los cambios se replican a través de las conexiones entre los miembros del grupo de replicación. Las conexiones entre todos los miembros conforman la topología de la replicación.
La creación de varias carpetas replicadas en un único grupo de replicación simplifica el proceso de implementación de las carpetas replicadas, ya que la topología, la programación y el límite de ancho de banda del grupo de replicación se aplican a cada una de las carpetas replicadas. Para implementar más carpetas replicadas, puedes usar Dfsradmin.exe o seguir las instrucciones de un asistente para definir la ruta de acceso local y los permisos de la nueva carpeta replicada.

Cada carpeta replicada también tiene una configuración única, en la que se incluyen los filtros de archivos y subcarpetas, de manera que puedes filtrar distintos archivos y subcarpetas para cada carpeta replicada.

Las carpetas replicadas almacenadas en cada miembro se pueden ubicar en distintos volúmenes del miembro y las carpetas replicadas no tienen que ser carpetas compartidas ni formar parte de un espacio de nombres. No obstante, el complemento Administración de DFS facilita el uso compartido de las carpetas replicadas y su publicación opcional en un espacio de nombres existente.

Puedes administrar la replicación DFS mediante la Administración de DFS, los comandos DfsrAdmin y Dfsrdiag o los scripts que llaman a WMI.

## <a name="requirements"></a>Requisitos

Para poder implementar Replicación DFS, debe configurar los servidores del modo siguiente:

- Actualiza el esquema de Active Directory Domain Services (AD DS) para incluir las adiciones de esquemas de Windows Server 2003 R2 o posterior. No puedes usar carpetas replicadas de solo lectura con las adiciones de esquema de Windows Server 2003 R2 o anterior.
- Asegúrese de que todos los servidores de un grupo de replicación se encuentran en el mismo bosque. No se puede habilitar la replicación en servidores de distintos bosques.
- Instale Replicación DFS en todos los servidores que actuarán como miembros de un grupo de replicación.
- Póngase en contacto con el proveedor del software antivirus para comprobar si es compatible con Replicación DFS.
- Busque las carpetas que desee replicar en volúmenes formateados con el sistema de archivos NTFS. Replicación DFS no admite el Sistema de archivos resistente (ReFS) ni el sistema de archivos FAT. Replicación DFS tampoco admite el contenido de replicación almacenado en volúmenes compartidos de clúster.

## <a name="interoperability-with-azure-virtual-machines"></a>Interoperabilidad con máquinas virtuales de Azure

Se ha probado el uso de la replicación DFS en una máquina virtual en Azure con Windows Server; sin embargo, hay algunas limitaciones y requisitos que se deben seguir.

- El uso de instantáneas o estados guardados para restaurar un servidor que ejecuta Replicación DFS para la replicación de todo salvo la carpeta SYSVOL hace que Replicación DFS no pueda llevarse a cabo, lo que requiere pasos de recuperación de bases de datos especiales. De forma similar, no exportes, clones ni copies las máquinas virtuales. Para obtener más información, consulte el artículo [2517913](https://support.microsoft.com/kb/2517913) de Microsoft Knowledge Base y [Virtualización segura de DFSR](https://techcommunity.microsoft.com/t5/storage-at-microsoft/safely-virtualizing-dfsr/ba-p/424671).
- Cuando realices copias de seguridad de los datos en una carpeta replicada hospedada en una máquina virtual, debes usar software de copia de seguridad de la máquina virtual invitada.
- La replicación DFS requiere acceso a los controladores de dominio físicos o virtualizados: no se puede comunicar directamente con Azure AD.
- Replicación DFS requiere una conexión VPN entre los miembros del grupo de replicación local y cualquier miembro hospedado en máquinas virtuales de Azure. También deberás configurar el enrutador local (por ejemplo, Forefront Threat Management Gateway) para permitir el asignador de extremos de RPC (puerto 135) y un puerto asignado de forma aleatoria entre 49152 y 65535 para pasar a través de la conexión VPN. Puedes usar el cmdlet Set-DfsrMachineConfiguration o la herramienta de línea de comandos Dfsrdiag para especificar un puerto estático en lugar del puerto aleatorio. Para obtener más información sobre cómo especificar un puerto estático para la Replicación DFS, consulte [Set DfsrServiceConfiguration](/powershell/module/dfsr/set-dfsrserviceconfiguration). Para obtener información sobre los puertos relacionados que deben abrirse para administrar Windows Server, consulte el artículo [832017](https://support.microsoft.com/kb/832017) en Microsoft Knowledge Base.

Para obtener una introducción sobre las máquinas virtuales de Azure, visite el [sitio web de Microsoft Azure](/azure/virtual-machines/).

## <a name="installing-dfs-replication"></a>Instalación de Replicación DFS

Replicación DFS es parte del rol Servicios de archivos y almacenamiento. Las herramientas de administración de DFS (Administración de DFS, el módulo Replicación DFS para Windows PowerShell y las herramientas de línea de comandos) se instalan por separado como parte de las Herramientas de administración remota del servidor.

Instala Replicación DFS mediante [Windows Admin Center](../../manage/windows-admin-center/overview.md), el Administrador del servidor o PowerShell, tal y como se describe en las secciones siguientes.

### <a name="to-install-dfs-by-using-server-manager"></a>Para instalar DFS mediante el Administrador del servidor

1. Abra el Administrador del servidor, haga clic en **Administrar**y, a continuación, en **Agregar roles y características**. Aparece el Asistente para agregar roles y características.

2. En la página **Selección de servidor** , seleccione el servidor o el disco duro virtual (VHD) de una máquina virtual sin conexión en la que desee instalar DFS.

3. Seleccione los servicios de rol y las características que desee instalar.

    - Para instalar el servicio Replicación DFS, en la página **Roles de servidor**, selecciona **Replicación DFS**.

    - Para instalar solamente las Herramientas de administración de DFS, en la página **Características** , expanda **Herramientas de administración remota del servidor**, **Herramientas de administración de roles**, expanda **Herramientas de Servicios de archivo**y, a continuación, seleccione **Herramientas de administración de DFS**.

         **Herramientas de administración de DFS** instala el complemento Administración de DFS, los módulos Replicación DFS y Espacios de nombres DFS para Windows PowerShell y las herramientas de línea de comandos, pero no instala ningún servicio DFS en el servidor.

### <a name="to-install-dfs-replication-by-using-windows-powershell"></a>Para instalar Replicación DFS mediante Windows PowerShell

Abre una sesión de Windows PowerShell con permisos de usuario elevados y, a continuación, escribe el siguiente comando, donde <name\> es el servicio de rol o la características que quieres instalar (consulta la siguiente tabla para ver una lista de los nombres de servicios de rol o características pertinentes):

```PowerShell
Install-WindowsFeature <name>
```

|Servicio de rol o característica|Nombre|
|---|---|
|Replicación DFS|`FS-DFS-Replication`|
|Herramientas de administración de DFS|`RSAT-DFS-Mgmt-Con`|

Por ejemplo, para instalar la parte de Herramientas del sistema de archivos distribuido de la característica Herramientas de administración remota del servidor, escriba:

```PowerShell
Install-WindowsFeature "RSAT-DFS-Mgmt-Con"
```

Para instalar las partes Replicación DFS y Herramientas del sistema de archivos distribuido de la característica Herramientas de administración remota del servidor, escribe:

```PowerShell
Install-WindowsFeature "FS-DFS-Replication", "RSAT-DFS-Mgmt-Con"
```

## <a name="additional-references"></a>Referencias adicionales

- [Introducción a Espacios de nombres DFS y Replicación DFS](/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/jj127250(v%3dws.11))
- [Lista de comprobación: implementación de la replicación DFS](/previous-versions/windows/it-pro/windows-server-2008-r2-and-2008/cc772201(v%3dws.11))
- [Lista de comprobación: administración de la replicación DFS](/previous-versions/windows/it-pro/windows-server-2008-r2-and-2008/cc755035(v%3dws.11))
- [Implementación de Replicación DFS](/previous-versions/windows/it-pro/windows-server-2008-r2-and-2008/cc770925(v%3dws.11))
- [Administración de Replicación DFS](/previous-versions/windows/it-pro/windows-server-2008-r2-and-2008/cc770925(v%3dws.11))
- [Solución de problemas de la replicación DFS](/previous-versions/windows/it-pro/windows-server-2008-r2-and-2008/cc732802(v%3dws.11))
