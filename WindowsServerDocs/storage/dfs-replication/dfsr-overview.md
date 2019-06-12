---
Title: Introducción a la replicación DFS
ms.date: 03/08/2019
ms.prod: windows-server-threshold
ms.technology: storage
author: JasonGerend
manager: elizapo
ms.author: jgerend
ms.openlocfilehash: dd381c04b02889a7f2e7b8992ff6050d1b0f078a
ms.sourcegitcommit: 48bb3e5c179dc520fa879b16c9afe09e07c87629
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/31/2019
ms.locfileid: "66453065"
---
# <a name="dfs-replication-overview"></a>Introducción a la replicación DFS

> Se aplica a: Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2, Windows Server 2008, Windows Server (canal semianual)

La replicación DFS es un servicio de rol en Windows Server que permite replicar carpetas (incluidos los que hace referencia a una ruta de acceso del espacio de nombres DFS) de manera eficaz en varios servidores y sitios. la replicación DFS es un motor de replicación con varios maestros eficaz que sirve para mantener las carpetas sincronizadas entre los servidores en las conexiones de red con ancho de banda limitado. Reemplaza el servicio de replicación de archivos (FRS) como el motor de replicación para espacios de nombres DFS, así como para replicar la carpeta SYSVOL de Active Directory Domain Services (AD DS) en dominios que usen el Windows Server 2008 o versiones posteriores nivel funcional de dominio.

Replicación DFS usa un algoritmo de compresión denominado compresión diferencial remota (RDC). RDC detecta los cambios en los datos de un archivo y permite que la replicación DFS replique sólo los bloques de archivo modificados en lugar del archivo completo.

Para obtener más información acerca de la replicación de SYSVOL mediante la replicación DFS, consulte [migrar la replicación de SYSVOL a DFS Replication](migrate-sysvol-to-dfsr.md).

Para usar la replicación DFS, debe crear grupos de replicación y agregar carpetas replicadas a los grupos. En la ilustración siguiente se muestran los grupos de replicación, las carpetas replicadas y los miembros.

![Un grupo de replicación que contiene una conexión entre dos miembros, cada uno con un par de carpetas replicadas](media/dfsr-overview.gif)

Esta ilustración muestra que un grupo de replicación es un conjunto de servidores, llamados miembros, que participa en la replicación de una o varias carpetas replicadas. Una carpeta replicada es una carpeta que se mantiene sincronizada en cada miembro. En la ilustración, hay dos carpetas replicadas: Proyectos y propuestas. Cuando cambian los datos en cada carpeta replicada, los cambios se replican a través de conexiones entre los miembros del grupo de replicación. Las conexiones entre todos los miembros forman la topología de replicación.
Creación de varias carpetas replicadas en un único grupo de replicación simplifica el proceso de implementación de carpetas replicadas porque la topología, la programación y el ancho de banda para el grupo de replicación se aplican a cada carpeta replicada. Para implementar más carpetas replicadas, puede usar Dfsradmin.exe o seguir las instrucciones en un Asistente para definir la ruta de acceso local y los permisos para la nueva carpeta replicada.

Cada carpeta replicada tiene una configuración única, como filtros, para que pueda filtrar distintos archivos y subcarpetas para cada carpeta replicada.

Las carpetas replicadas almacenadas en cada miembro se pueden ubicar en distintos volúmenes del miembro y las carpetas replicadas no es necesario para las carpetas compartidas o parte de un espacio de nombres. Sin embargo, el complemento de administración de DFS facilita compartir carpetas replicadas y su publicación opcional en un espacio de nombres existente.

Puede administrar la replicación DFS con administración de DFS, los comandos DfsrAdmin y Dfsrdiag o scripts que llaman a WMI.

## <a name="requirements"></a>Requisitos

Para poder implementar Replicación DFS, debe configurar los servidores del modo siguiente:

- Actualice el esquema de Active Directory Domain Services (AD DS) para incluir Windows Server 2003 R2 o posterior adiciones de esquema. No se puede usar carpetas replicadas de solo lectura con el Windows Server 2003 R2 o adiciones de esquema anterior.
- Asegúrese de que todos los servidores de un grupo de replicación se encuentran en el mismo bosque. No se puede habilitar la replicación en servidores de distintos bosques.
- Instale Replicación DFS en todos los servidores que actuarán como miembros de un grupo de replicación.
- Póngase en contacto con el proveedor del software antivirus para comprobar si es compatible con Replicación DFS.
- Busque las carpetas que desee replicar en volúmenes formateados con el sistema de archivos NTFS. Replicación DFS no admite el Sistema de archivos resistente (ReFS) ni el sistema de archivos FAT. Replicación DFS tampoco admite el contenido de replicación almacenado en volúmenes compartidos de clúster.

## <a name="interoperability-with-azure-virtual-machines"></a>Interoperabilidad con máquinas virtuales de Azure

Mediante la replicación DFS en una máquina virtual en Azure se ha probado con Windows Server; Sin embargo, hay algunas limitaciones y requisitos que debe seguir.

- El uso de instantáneas o estados guardados para restaurar un servidor que ejecuta Replicación DFS para la replicación de todo salvo la carpeta SYSVOL hace que Replicación DFS no pueda llevarse a cabo, lo que requiere pasos de recuperación de bases de datos especiales. De forma similar, no exporte, clone ni copie las máquinas virtuales. Para obtener más información, consulte el artículo [2517913](http://support.microsoft.com/kb/2517913) de Microsoft Knowledge Base y [Virtualización segura de DFSR](https://blogs.technet.microsoft.com/filecab/2013/04/05/safely-virtualizing-dfsr/).
- Cuando realice copias de seguridad de los datos en una carpeta replicada hospedada en una máquina virtual, deberá usar software de copia de seguridad de la máquina virtual invitada.
- La replicación DFS requiere acceso a los controladores de dominio físicos o virtualizados: no puede comunicarse directamente con Azure AD.
- Replicación DFS requiere una conexión VPN entre los miembros del grupo de replicación local y cualquier miembro hospedado en máquinas virtuales de Azure. También deberá configurar el enrutador local en (por ejemplo, Forefront Threat Management Gateway) para permitir el asignador de extremos de RPC (el puerto 135) y un puerto asignado de forma aleatoria entre 49152 y 65535 para pasar a través de la conexión VPN. Puede usar el cmdlet Set-DfsrMachineConfiguration o la herramienta de línea de comandos Dfsrdiag para especificar un puerto estático en lugar del puerto aleatorio. Para obtener más información sobre cómo especificar un puerto estático para la Replicación DFS, consulte [Set DfsrServiceConfiguration](https://docs.microsoft.com/powershell/module/dfsr/set-dfsrserviceconfiguration). Para obtener información sobre los puertos relacionados que deben abrirse para administrar Windows Server, consulte el artículo [832017](http://support.microsoft.com/kb/832017) en Microsoft Knowledge Base.

Para obtener una introducción sobre las máquinas virtuales de Azure, visite el [sitio web de Microsoft Azure](https://docs.microsoft.com/azure/virtual-machines/).

## <a name="installing-dfs-replication"></a>Instalación de la replicación DFS

La replicación DFS es una parte de la función Servicios de archivos y almacenamiento. Las herramientas de administración de DFS (administración de DFS, el módulo de la replicación DFS para Windows PowerShell y herramientas de línea de comandos) se instalan por separado como parte de herramientas de administración remota del servidor.

Instalar la replicación DFS mediante [Windows Admin Center](../../manage/windows-admin-center/understand/windows-admin-center.md), el administrador del servidor o PowerShell, tal como se describe en las secciones siguientes.

### <a name="to-install-dfs-by-using-server-manager"></a>Para instalar DFS mediante el Administrador del servidor

1. Abra el Administrador del servidor, haga clic en **Administrar** y, a continuación, en **Agregar roles y características**. Aparece el Asistente para agregar roles y características.

2. En la página **Selección de servidor**, seleccione el servidor o el disco duro virtual (VHD) de una máquina virtual sin conexión en la que desee instalar DFS.

3. Seleccione los servicios de rol y las características que desee instalar.

    - Para instalar el servicio de replicación DFS, en el **Roles de servidor** página, seleccione **replicación DFS**.

    - Para instalar solamente las Herramientas de administración de DFS, en la página **Características**, expanda **Herramientas de administración remota del servidor**, **Herramientas de administración de roles**, expanda **Herramientas de Servicios de archivo** y, a continuación, seleccione **Herramientas de administración de DFS**.

         **Las herramientas de administración de DFS** instala la administración de DFS complemento, la replicación DFS y los módulos de espacios de nombres DFS para Windows PowerShell y herramientas de línea de comandos, pero no instala ningún servicio DFS en el servidor.

### <a name="to-install-dfs-replication-by-using-windows-powershell"></a>Para instalar la replicación DFS mediante Windows PowerShell

Abra una sesión de Windows PowerShell con derechos de usuario con privilegios elevados y, a continuación, escriba el siguiente comando, donde < nombre\> es el servicio de rol o característica que desea instalar (consulte la tabla siguiente para obtener una lista de nombres de característica o servicio de rol relevantes):

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

Para instalar la replicación DFS y las partes de herramientas del sistema de archivos distribuido de la característica herramientas de administración remota del servidor, escriba:

```PowerShell
Install-WindowsFeature "FS-DFS-Replication", "RSAT-DFS-Mgmt-Con"
```

## <a name="see-also"></a>Vea también

- [Información general sobre espacios de nombres DFS y replicación DFS](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/jj127250(v%3dws.11))
- [Lista de comprobación: Implementar la replicación DFS](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc772201(v%3dws.11))
- [Lista de comprobación: Administrar la replicación DFS](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc755035(v%3dws.11))
- [Implementación de la replicación DFS](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc770925(v%3dws.11))
- [Administrar la replicación DFS](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc770925(v%3dws.11))
- [Solución de problemas de replicación DFS](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc732802(v%3dws.11))
