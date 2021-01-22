---
title: Información general sobre espacios de nombres DFS
ms.author: jgerend
manager: daveba
ms.topic: article
author: jasongerend
ms.date: 01/21/2021
description: En este tema se describen los espacios de nombres DFS, que es un servicio de rol de Windows Server que permite agrupar las carpetas compartidas ubicadas en distintos servidores en uno o más espacios de nombres estructurados lógicamente.
ms.openlocfilehash: ffc909f4feb23d26cf496eddca51899a97f4cafe
ms.sourcegitcommit: fc2a7c69a74edcd79372054c4a9a24237510babd
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/22/2021
ms.locfileid: "98672947"
---
# <a name="dfs-namespaces-overview"></a>Información general sobre espacios de nombres DFS

> Se aplica a: Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2, Windows Server 2008, Windows Server (canal semianual)

Espacios de nombres DFS es un servicio de rol de Windows Server que permite agrupar las carpetas compartidas ubicadas en distintos servidores en uno o más espacios de nombres estructurados lógicamente. Esto permite proporcionar a los usuarios una vista virtual de las carpetas compartidas, donde una sola ruta de acceso conduce a archivos ubicados en varios servidores, como se muestra en la ilustración siguiente:

![Elementos de la tecnología de espacios de nombres DFS](media/dfs-overview.png)

Esta es una descripción de los elementos que componen un espacio de nombres DFS:

- **Servidor de espacio de nombres** : un servidor de espacio de nombres hospeda un espacio de nombres. El servidor de espacio de nombres puede ser un servidor miembro o un controlador de dominio.
- **Raíz** del espacio de nombres: la raíz del espacio de nombres es el punto inicial del espacio de nombres. En la ilustración anterior, el nombre de la raíz es público y la ruta de acceso del espacio de nombres es \\ \\ contoso \\ Public. Este tipo de espacio de nombres se conoce como espacio de nombres basado en dominio, ya que comienza con un nombre de dominio (por ejemplo, Contoso) y sus metadatos se almacenan en Active Directory Domain Services (AD DS). Aunque en la figura anterior se muestra un solo servidor de espacio de nombres, un espacio de nombres basado en dominio puede hospedarse en varios servidores de espacio de nombres para aumentar su disponibilidad.
- **Carpeta** : carpetas sin destinos de carpeta agregue la estructura y la jerarquía al espacio de nombres, y las carpetas con destinos de carpeta proporcionan a los usuarios contenido real. Cuando los usuarios examinan una carpeta con destinos de carpeta en el espacio de nombres, el equipo cliente recibe una referencia que lo redirige de forma transparente a uno de los destinos de carpeta.
- **Destinos de carpeta** : un destino de carpeta es la ruta de acceso UNC de una carpeta compartida o de otro espacio de nombres que está asociado a una carpeta en un espacio de nombres. El destino de carpeta en donde se almacenan los datos y el contenido. En la figura anterior, la carpeta llamada Tools tiene dos destinos de carpeta, uno en London y otro en New York, y la carpeta llamada Training Guides tiene un único destino de carpeta en New York. Un usuario que vaya a las \\ \\ \\ herramientas de software público de Contoso \\ \\ se redirige de forma transparente a las herramientas de la carpeta compartida \\ \\ LDN-SVR-01 o a las herramientas \\ \\ \\ Nueva York-SVR-01 \\ , según el sitio en el que se encuentre el usuario actualmente.

En este tema se describe cómo instalar DFS, las novedades y dónde encontrar información de evaluación e implementación.

Puede administrar los espacios de nombres mediante administración de DFS, los [cmdlets del espacio de nombres DFS (DFSN) en Windows PowerShell](/powershell/module/dfsn/), el comando **DfsUtil** o los scripts que llaman a WMI.

## <a name="server-requirements-and-limits"></a>Requisitos y límites del servidor

No hay requisitos de hardware ni software adicionales para ejecutar Administración de DFS o usar los espacios de nombres DFS.

Un servidor de espacio de nombres es un controlador de dominio o servidor miembro que hospeda un espacio de nombres. El número de espacios de nombres que se pueden hospedar en un servidor viene determinado por el sistema operativo que ejecute el servidor de espacio de nombres.

Los servidores que ejecutan los siguientes sistemas operativos pueden hospedar varios espacios de nombres basados en dominio además de un solo espacio de nombres independiente.

- Windows Server 2019
- Windows Server 2016
- Windows Server 2012 R2
- Windows Server 2012
- Windows Server 2008 R2 Datacenter y Enterprise Edition
- Windows Server (Canal semianual)

Los servidores que ejecutan los siguientes sistemas operativos pueden hospedar un solo espacio de nombres independiente:

- Windows Server 2008 R2 Standard

En la siguiente tabla se describen otros factores que debe tener en cuenta al seleccionar los servidores que hospedarán un espacio de nombres.

| Servidor que hospeda espacios de nombres independientes | Servidor que hospeda espacios de nombres basados en dominios |
| ---                                   |        ---                                |
| Debe contener un volumen NTFS para hospedar el espacio de nombres.|Debe contener un volumen NTFS para hospedar el espacio de nombres. |
| Puede ser un servidor miembro o controlador de dominio.|Debe ser un servidor miembro o controlador de dominio en el dominio donde se configura el espacio de nombres. (Este requisito se aplica a todos los servidores de espacio de nombres que hospedan un espacio de nombres basado en dominio en concreto.) |
| Lo puede hospedar un clúster de conmutación por error para aumentar la disponibilidad del espacio de nombres.|El espacio de nombres no puede ser un recurso de clúster en un clúster de conmutación por error. No obstante, puede ubicar el espacio de nombres en un servidor que también funcione como nodo en un clúster de conmutación por error si configura el espacio de nombres para que use solamente los recursos locales de ese servidor. |

## <a name="installing-dfs-namespaces"></a>Instalación de espacios de nombres DFS

Espacios de nombres DFS y Replicación DFS forman parte del rol Servicios de archivos y almacenamiento. Las herramientas de administración de DFS (Administración de DFS, el módulo Espacios de nombres DFS para Windows PowerShell y las herramientas de línea de comandos) se instalan por separado como parte de las Herramientas de administración remota del servidor.

Instale espacios de nombres DFS mediante el [centro de administración de Windows](../../manage/windows-admin-center/overview.md), administrador del servidor o PowerShell, tal como se describe en las secciones siguientes.

### <a name="to-install-dfs-by-using-server-manager"></a>Para instalar DFS mediante el Administrador del servidor

1. Abra el Administrador del servidor, haga clic en **Administrar** y, a continuación, en **Agregar roles y características**. Aparece el Asistente para agregar roles y características.

2. En la página **Selección de servidor** , seleccione el servidor o el disco duro virtual (VHD) de una máquina virtual sin conexión en la que desee instalar DFS.

3. Seleccione los servicios de rol y las características que desee instalar.

    - Para instalar el servicio de espacios de nombres DFS, en la página **roles de servidor** , seleccione **espacios de nombres DFS**.

    - Para instalar solamente las Herramientas de administración de DFS, en la página **Características** , expanda **Herramientas de administración remota del servidor**, **Herramientas de administración de roles**, expanda **Herramientas de Servicios de archivo** y, a continuación, seleccione **Herramientas de administración de DFS**.

         **Herramientas de administración de DFS** instala el complemento Administración de DFS, el módulo Espacios de nombres DFS para Windows PowerShell y las herramientas de línea de comandos, pero no instala ningún servicio DFS en el servidor.

### <a name="to-install-dfs-by-using-windows-powershell"></a>Para instalar DFS mediante Windows PowerShell

Abre una sesión de Windows PowerShell con permisos de usuario elevados y, a continuación, escribe el siguiente comando, donde <name\> es el servicio de rol o la características que quieres instalar (consulta la siguiente tabla para ver una lista de los nombres de servicios de rol o características pertinentes):

```PowerShell
Install-WindowsFeature <name>
```

| Servicio de rol o característica | Nombre |
| ----------------------- | ---- |
| Espacios de nombres DFS          | `FS-DFS-Namespace` |
| Herramientas de administración de DFS    | `RSAT-DFS-Mgmt-Con` |

Por ejemplo, para instalar la parte de Herramientas del sistema de archivos distribuido de la característica Herramientas de administración remota del servidor, escriba:

```PowerShell
Install-WindowsFeature "RSAT-DFS-Mgmt-Con"
```

Para instalar los espacios de nombres DFS y las partes de herramientas de Sistema de archivos distribuido de la característica Herramientas de administración remota del servidor, escriba:

```PowerShell
Install-WindowsFeature "FS-DFS-Namespace", "RSAT-DFS-Mgmt-Con"
```

## <a name="interoperability-with-azure-virtual-machines"></a>Interoperabilidad con máquinas virtuales de Azure

Se ha probado el uso de espacios de nombres DFS en una máquina virtual en Microsoft Azure.

- Puede hospedar espacios de nombres basados en dominio en máquinas virtuales de Azure, incluidos los entornos con Azure Active Directory.
- Puede agrupar los espacios de nombres independientes en máquinas virtuales de Azure mediante clústeres de conmutación por error que usan discos [compartidos](/azure/virtual-machines/disks-shared) o [discos ultra](/azure/virtual-machines/disks-enable-ultra-ssd).


Para obtener información sobre cómo empezar a trabajar con Azure virtual machines, consulte la [documentación de Azure virtual machines](/azure/virtual-machines/).

## <a name="additional-references"></a>Referencias adicionales

Para obtener más información relacionada, vea los siguientes recursos.

| Tipo de contenido        | Referencias |
| ------------------  | ----------------|
| **Evaluación del producto** | [Novedades de los espacios de nombres DFS y Replicación DFS en Windows Server](/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/dn281957(v=ws.11)) |
| **Implementación**    | [Consideraciones sobre la escalabilidad de espacios de nombres DFS](https://techcommunity.microsoft.com/t5/storage-at-microsoft/bg-p/FileCAB) |
| **Operaciones**    | [Espacios de nombres DFS: preguntas más frecuentes](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/ee404780(v=ws.10)) |
| **Recursos de la comunidad** | [Foro de TechNet sobre servicios de archivos y almacenamiento](/answers/topics/windows-server-storage.html) |
| **Protocolos**        | [Protocolos de servicios de archivo en Windows Server](/openspecs/windows_protocols/MS-WINPROTLP/df36f95e-6a6b-48d6-a3ae-35a17674f546) (desusado) |
| **Tecnologías relacionadas** | [Clúster de conmutación por error](../../failover-clustering/failover-clustering-overview.md)|
| **Soporte técnico** | [Soporte técnico Windows IT Pro](https://www.microsoft.com/itpro/windows/support)|
