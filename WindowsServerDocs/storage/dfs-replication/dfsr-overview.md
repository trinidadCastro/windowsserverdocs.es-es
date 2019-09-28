---
title: Información general de Replicación DFS
ms.date: 03/08/2019
ms.prod: windows-server
ms.technology: storage
author: JasonGerend
manager: elizapo
ms.author: jgerend
ms.openlocfilehash: eebce26eef6eceddc064e3bb179f268ccf47c93d
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71386063"
---
# <a name="dfs-replication-overview"></a>Información general de Replicación DFS

> Se aplica a: Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2, Windows Server 2008, Windows Server (canal semianual)

Replicación DFS es un servicio de rol de Windows Server que permite replicar carpetas de forma eficaz (incluidas las a las que hace referencia una ruta de acceso de espacio de nombres DFS) en varios servidores y sitios. la replicación DFS es un motor de replicación con varios maestros eficaz que sirve para mantener las carpetas sincronizadas entre los servidores en las conexiones de red con ancho de banda limitado. Reemplaza el servicio de replicación de archivos (FRS) como motor de replicación para los espacios de nombres DFS, así como para replicar la carpeta SYSVOL Active Directory Domain Services (AD DS) en dominios que usan el nivel funcional de dominio de Windows Server 2008 o posterior.

Replicación DFS usa un algoritmo de compresión denominado compresión diferencial remota (RDC). RDC detecta los cambios en los datos de un archivo y permite que la replicación DFS replique sólo los bloques de archivo modificados en lugar del archivo completo.

Para obtener más información acerca de la replicación de SYSVOL mediante Replicación DFS, vea [migrar la replicación de SYSVOL a replicación DFS](migrate-sysvol-to-dfsr.md).

Para usar Replicación DFS, debe crear grupos de replicación y agregar carpetas replicadas a los grupos. Los grupos de replicación, las carpetas replicadas y los miembros se muestran en la ilustración siguiente.

![Un grupo de replicación que contiene una conexión entre dos miembros, cada uno con un par de carpetas replicadas.](media/dfsr-overview.gif)

En esta ilustración se muestra que un grupo de replicación es un conjunto de servidores, denominados miembros, que participa en la replicación de una o más carpetas replicadas. Una carpeta replicada es una carpeta que permanece sincronizada en cada miembro. En la ilustración, hay dos carpetas replicadas: Proyectos y propuestas. A medida que los datos cambian en cada carpeta replicada, los cambios se replican entre las conexiones entre los miembros del grupo de replicación. Las conexiones entre todos los miembros forman la topología de replicación.
La creación de varias carpetas replicadas en un único grupo de replicación simplifica el proceso de implementación de carpetas replicadas porque la topología, la programación y la limitación de ancho de banda para el grupo de replicación se aplican a cada carpeta replicada. Para implementar carpetas replicadas adicionales, puede usar Dfsradmin. exe o seguir las instrucciones de un asistente para definir la ruta de acceso local y los permisos de la nueva carpeta replicada.

Cada carpeta replicada tiene una configuración única, como filtros de archivos y subcarpetas, para que pueda filtrar distintos archivos y subcarpetas para cada carpeta replicada.

Las carpetas replicadas almacenadas en cada miembro se pueden ubicar en diferentes volúmenes del miembro y las carpetas replicadas no necesitan ser carpetas compartidas o parte de un espacio de nombres. Sin embargo, el complemento Administración de DFS facilita el uso compartido de las carpetas replicadas y su publicación opcional en un espacio de nombres existente.

Puede administrar Replicación DFS mediante administración de DFS, los comandos DfsrAdmin y Dfsrdiag, o los scripts que llaman a WMI.

## <a name="requirements"></a>Requisitos

Para poder implementar Replicación DFS, debe configurar los servidores del modo siguiente:

- Actualice el esquema de Active Directory Domain Services (AD DS) para incluir las adiciones de esquema de Windows Server 2003 R2 o posterior. No puede usar carpetas replicadas de solo lectura con las adiciones de esquema de Windows Server 2003 R2 o anterior.
- Asegúrese de que todos los servidores de un grupo de replicación se encuentran en el mismo bosque. No se puede habilitar la replicación en servidores de distintos bosques.
- Instale Replicación DFS en todos los servidores que actuarán como miembros de un grupo de replicación.
- Póngase en contacto con el proveedor del software antivirus para comprobar si es compatible con Replicación DFS.
- Busque las carpetas que desee replicar en volúmenes formateados con el sistema de archivos NTFS. Replicación DFS no admite el Sistema de archivos resistente (ReFS) ni el sistema de archivos FAT. Replicación DFS tampoco admite el contenido de replicación almacenado en volúmenes compartidos de clúster.

## <a name="interoperability-with-azure-virtual-machines"></a>Interoperabilidad con máquinas virtuales de Azure

El uso de Replicación DFS en una máquina virtual de Azure se ha probado con Windows Server; sin embargo, hay algunas limitaciones y requisitos que debe seguir.

- El uso de instantáneas o estados guardados para restaurar un servidor que ejecuta Replicación DFS para la replicación de todo salvo la carpeta SYSVOL hace que Replicación DFS no pueda llevarse a cabo, lo que requiere pasos de recuperación de bases de datos especiales. Del mismo modo, no exporte, Clone ni Copie las máquinas virtuales. Para obtener más información, consulte el artículo [2517913](http://support.microsoft.com/kb/2517913) de Microsoft Knowledge Base y [Virtualización segura de DFSR](https://blogs.technet.microsoft.com/filecab/2013/04/05/safely-virtualizing-dfsr/).
- Cuando realice copias de seguridad de los datos en una carpeta replicada hospedada en una máquina virtual, deberá usar software de copia de seguridad de la máquina virtual invitada.
- Replicación DFS requiere acceso a los controladores de dominio físicos o virtualizados: no se puede comunicar directamente con Azure AD.
- Replicación DFS requiere una conexión VPN entre los miembros del grupo de replicación local y cualquier miembro hospedado en máquinas virtuales de Azure. También deberá configurar el enrutador local en (por ejemplo, Forefront Threat Management Gateway) para permitir el asignador de extremos de RPC (el puerto 135) y un puerto asignado de forma aleatoria entre 49152 y 65535 para pasar a través de la conexión VPN. Puede usar el cmdlet Set-DfsrMachineConfiguration o la herramienta de línea de comandos Dfsrdiag para especificar un puerto estático en lugar del puerto aleatorio. Para obtener más información sobre cómo especificar un puerto estático para la Replicación DFS, consulte [Set DfsrServiceConfiguration](https://docs.microsoft.com/powershell/module/dfsr/set-dfsrserviceconfiguration). Para obtener información sobre los puertos relacionados que deben abrirse para administrar Windows Server, consulte el artículo [832017](http://support.microsoft.com/kb/832017) en Microsoft Knowledge Base.

Para obtener una introducción sobre las máquinas virtuales de Azure, visite el [sitio web de Microsoft Azure](https://docs.microsoft.com/azure/virtual-machines/).

## <a name="installing-dfs-replication"></a>Instalación de Replicación DFS

Replicación DFS forma parte del rol servicios de archivos y almacenamiento. Las herramientas de administración de DFS (administración de DFS, el módulo de Replicación DFS para Windows PowerShell y las herramientas de línea de comandos) se instalan por separado como parte de la Herramientas de administración remota del servidor.

Instale Replicación DFS mediante el [centro de administración de Windows](../../manage/windows-admin-center/understand/windows-admin-center.md), administrador del servidor o PowerShell, tal como se describe en las secciones siguientes.

### <a name="to-install-dfs-by-using-server-manager"></a>Para instalar DFS mediante el Administrador del servidor

1. Abra el Administrador del servidor, haga clic en **Administrar** y, a continuación, en **Agregar roles y características**. Aparece el Asistente para agregar roles y características.

2. En la página **Selección de servidor**, seleccione el servidor o el disco duro virtual (VHD) de una máquina virtual sin conexión en la que desee instalar DFS.

3. Seleccione los servicios de rol y las características que desee instalar.

    - Para instalar el servicio Replicación DFS, en la página **roles de servidor** , seleccione **replicación DFS**.

    - Para instalar solamente las Herramientas de administración de DFS, en la página **Características**, expanda **Herramientas de administración remota del servidor**, **Herramientas de administración de roles**, expanda **Herramientas de Servicios de archivo** y, a continuación, seleccione **Herramientas de administración de DFS**.

         **Herramientas de administración de DFS** instala el complemento Administración de DFS, los módulos replicación DFS y espacios de nombres DFS para Windows PowerShell y las herramientas de línea de comandos, pero no instala ningún servicio DFS en el servidor.

### <a name="to-install-dfs-replication-by-using-windows-powershell"></a>Para instalar Replicación DFS mediante Windows PowerShell

Abra una sesión de Windows PowerShell con permisos de usuario elevados y, a continuación, escriba el siguiente\> comando, donde < nombre es el servicio de rol o la característica que desea instalar (vea la tabla siguiente para obtener una lista de los nombres de servicio de rol o de características relevantes):

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

Para instalar el Replicación DFS y las partes Sistema de archivos distribuido herramientas de la característica Herramientas de administración remota del servidor, escriba:

```PowerShell
Install-WindowsFeature "FS-DFS-Replication", "RSAT-DFS-Mgmt-Con"
```

## <a name="see-also"></a>Vea también

- [Información general sobre espacios de nombres DFS y Replicación DFS](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/jj127250(v%3dws.11))
- [Lista de comprobación: Implementar Replicación DFS](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc772201(v%3dws.11))
- [Lista de comprobación: Administrar Replicación DFS](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc755035(v%3dws.11))
- [Implementación de Replicación DFS](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc770925(v%3dws.11))
- [Administrar Replicación DFS](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc770925(v%3dws.11))
- [Solución de problemas Replicación DFS](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc732802(v%3dws.11))
