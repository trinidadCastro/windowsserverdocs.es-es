---
title: Instalación de servidor con Experiencia de escritorio
description: 'Se explica cómo obtener e instalar un servidor con Experiencia de escritorio '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.date: 01/18/2017
ms.technology: server-general
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 5b38b8a0-4dfc-4130-be00-fc58bba99595
author: jaimeo
ms.author: jaimeo
manager: dongill
ms.localizationpriority: medium
ms.openlocfilehash: cf67a1c9675191936a6150bb950c59e6f99b54ad
ms.sourcegitcommit: 3743cf691a984e1d140a04d50924a3a0a19c3e5c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/17/2019
ms.locfileid: "66810695"
---
# <a name="install-server-with-desktop-experience"></a>Instalación de servidor con Experiencia de escritorio
> Se aplica a: Windows Server 2016
  

Cuando se instala Windows Server 2016 mediante el Asistente para la instalación, puede elegir entre **Windows Server 2016** y **Windows Server 2016 (Servidor con Experiencia de escritorio)** . La opción Servidor con Experiencia de escritorio es el equivalente de Windows Server 2016 de la opción de instalación completa disponible en Windows Server 2012 R2 con la característica Experiencia de escritorio instalada. Si no elige una opción en el Asistente para instalación, se instala **Windows Server 2016**; se trata de la opción de instalación **Server Core**.

La opción Servidor con Experiencia de escritorio instala la interfaz de usuario estándar y todas las herramientas, incluidas las características de experiencia de cliente que requieren una instalación independiente de Windows Server 2012 R2. Los roles y características de servidor se instalan con Administrador del servidor o con otros métodos. En comparación con la opción Server Core, requiere más espacio en disco y tiene requisitos de mantenimiento de mantenimiento más estrictos, por lo que se recomienda que elija la instalación Server Core a menos que necesite particularmente los elementos de la interfaz de usuario y las herramientas de administración de gráficos que se incluyen en la opción Servidor con Experiencia de escritorio. Si cree que puede trabajar sin los elementos adicionales, vea [Instalar Server Core](Getting-Started-with-Server-Core.md). Para ver una opción aún más ligera, consulte [Instalar Nano Server](Getting-Started-with-Nano-Server.md).

> [!NOTE]
>
> A diferencia de algunas versiones anteriores de Windows Server, no se puede convertir entre Server Core y Servidor con Experiencia de escritorio tras la instalación. Si instala Servidor con Experiencia de escritorio y más tarde decides usar Server Core, debes efectuar una instalación nueva.

**Interfaz de usuario:** Interfaz gráfica de usuario estándar (“Shell gráfico de servidor”). El Shell gráfico de servidor incluye el nuevo shell de Windows 10. Las características específicas de Windows instaladas de forma predeterminada con esta opción son User-Interfaces-Infra, Server-GUI-Shell, Server-GUI-Mgmt-Infra, InkAndHandwritingServices, ServerMediaFoundation y Experiencia de escritorio. Aunque estas características aparecen en el Administrador del servidor en esta versión, no se admite su desinstalación y no estarán disponibles en las versiones futuras.

**Instalar, configurar, desinstalar roles de servidor localmente:** con el Administrador del servidor o con Windows PowerShell.

**Instalar, configurar, desinstalar roles de servidor remotamente:** con el Administrador del servidor, RSAT o Windows PowerShell.

**Microsoft Management Console: instalada**

## <a name="installation-scenarios"></a>Escenarios de instalación

### <a name="evaluation"></a>Evaluación
Puede obtener una copia de evaluación con una licencia para 180 días de Windows Server desde [Evaluaciones de Windows Server](https://www.microsoft.com/evalcenter/evaluate-windows-server-2016). Elija la opción **Windows Server 2016 | ISO de 64 bits** para la descarga, o puede visitar **Windows Server 2016 | Laboratorio virtual**.

> [!IMPORTANT]  
> Para las versiones de Windows Server 2016 anteriores a 14393.0.161119-1705.RS1_REFRESH, solo puedes hacer esta conversión de evaluación a comercial con Windows Server 2016 instalado con la opción de Experiencia de escritorio (no la opción Server Core). A partir de la versión 14393.0.161119-1705.RS1_REFRESH y versiones posteriores, puedes convertir las ediciones de evaluación en comerciales, independientemente de la opción de instalación usada.


### <a name="clean-installation"></a>Instalación limpia

Para instalar la opción Servidor con Experiencia de escritorio desde un medio, inserte el medio en la unidad, reinicie el equipo y ejecute Setup.exe. En el asistente que se abre, seleccione **Windows Server (Servidor con Experiencia de escritorio)** (Standard o Datacenter) y luego complete el asistente.

### <a name="upgrade"></a>Actualizar versión
**Actualizar** significa pasar de la versión actual del sistema operativo a una versión más reciente sin cambiar de hardware.

Si ya tiene una instalación completa del producto Windows Server adecuado, puede actualizar a una instalación de Servidor con Experiencia de escritorio de la edición adecuada de Windows Server 2016, como se indica a continuación.

> [!IMPORTANT]  
> En esta versión, la actualización funciona mejor en máquinas virtuales donde los controladores de hardware específicos de OEM no son necesarios para una actualización correcta. De lo contrario, la migración es la opción recomendada.  

- No se admiten actualizaciones inmediatas de arquitecturas de 32 a 64 bits. Todas las ediciones de Windows Server 2016 son solo de 64 bits.
- No se admiten actualizaciones inmediatas de un idioma a otro.
- Si el servidor es un controlador de dominio, vea [Actualizar controladores de dominio a Windows Server 2012 R2 y Windows Server 2012](https://technet.microsoft.com/library/hh994618.aspx) para obtener información importante.
- No se admiten las actualizaciones desde versiones preliminares (vista preliminar) de Windows Server 2016. Realice una instalación limpia a Windows Server 2016.
- No se admiten actualizaciones que cambian de la instalación Server Core al modo Servidor con Experiencia de escritorio (o viceversa).

Si no ve su versión actual en la columna de la izquierda, significa que no se admite la actualización a esta versión de Windows Server 2016.

Si ve más de una edición en la columna de la derecha, se admite la actualización a **cualquier** edición desde la misma versión inicial.

|Si ejecuta esta edición:|Puede realizar una actualización a estas ediciones:|  
|-------------------|----------|  
|Windows Server 2012 Standard|Windows Server 2016 Standard o Datacenter|
|Windows Server 2012 Datacenter|Windows Server 2016 Datacenter|
|Windows Server 2012 R2 Standard|Windows Server 2016 Standard o Datacenter|
|Windows Server 2012 R2 Datacenter|Windows Server 2016 Datacenter|
|Windows Server 2012 R2 Essentials|Windows Server 2016 Essentials|
|Windows Storage Server 2012 Standard|Windows Storage Server 2016 Standard|
|Windows Storage Server 2012 Workgroup|Windows Storage Server 2016 Workgroup|
|Windows Storage Server 2012 R2 Standard|Windows Storage Server 2016 Standard|
|Windows Storage Server 2012 R2 Workgroup|Windows Storage Server 2016 Workgroup|

Para muchas opciones adicionales para migrar a Windows Server 2016, como la conversión de licencia entre ediciones de licencia por volumen, versiones de evaluación y otras, vea los detalles de las [opciones de actualización](Supported-Upgrade-Paths.md).

### <a name="migration"></a>Migración
**Migración** significa mover desde el sistema operativo existente a Windows Server 2016 mediante una instalación limpia en un conjunto diferente de hardware o máquina virtual y después transferir las cargas de trabajo del servidor anterior al nuevo servidor. La migración, que puede variar de forma notable según los roles de servidor que haya instalados, se aborda en profundidad en [Windows Server Installation, Upgrade, and Migration](https://technet.microsoft.com/windowsserver/dn458795) (Instalación, actualización y migración de Windows Server).

La capacidad de migración varía entre los diferentes roles de servidor. En la cuadrícula siguiente se explican las opciones de actualización y migración de roles de servidor específicamente para migrar a Windows Server 2016. Para obtener instrucciones para la migración de roles individuales, visite [Migración de roles y características en Windows Server](https://technet.microsoft.com/windowsserver/jj554790.aspx). Para más información sobre la instalación y las actualizaciones, vea [Windows Server Installation, Upgrade, and Migration](https://technet.microsoft.com/windowsserver/dn458795) (Instalación, actualización y migración de Windows Server).

|Rol de servidor|¿Se puede actualizar desde Windows Server 2012 R2?|¿Se puede actualizar desde Windows Server 2012?|¿Se admite la migración?|¿La migración puede completarse sin tiempo de inactividad?|  
|-------------------|----------|--------------|--------------|----------|  
|Servicios de certificados de Active Directory| Sí|    Sí|    Sí|    No|
|Active Directory Domain Services|  Sí|    Sí|    Sí|    Sí|
|Servicios de federación de Active Directory (AD FS)|  No| No| Sí|    No (es necesario agregar nuevos nodos a la granja)|
|Active Directory Lightweight Directory Services|   Sí|    Sí|    Sí|    Sí|
|Active Directory Rights Management Services|   Sí|    Sí|    Sí|    No|
|Clúster de conmutación por error|Sí, con el proceso de [Cluster OS Rolling Upgrade](https://technet.microsoft.com/windows-server-docs/failover-clustering/cluster-operating-system-rolling-upgrade) (Actualización gradual de sistema operativo de clúster) que incluye el nodo Pause-Drain, Evict, actualizar a Windows Server 2016 y volver a unir el clúster original. Sí, cuando el servidor se quita del clúster para la actualización y después se agrega a un clúster diferente.|No mientras el servidor forma parte de un clúster. Sí, cuando el servidor se quita del clúster para la actualización y después se agrega a un clúster diferente.  |Sí|No para clústeres de conmutación por error en Windows Server 2012. Sí, para clústeres de conmutación por error de Windows Server 2012 R2 con máquinas virtuales de Hyper-V o clústeres de conmutación por error de Windows Server 2012 R2 que ejecutan el rol de Servidor de archivos de escalabilidad horizontal. Vea [Cluster OS Rolling Upgrade](https://technet.microsoft.com/windows-server-docs/failover-clustering/cluster-operating-system-rolling-upgrade) (Actualización gradual de sistema operativo de clúster).|
|Servicios de archivos y almacenamiento| Sí|    Sí|    Varía según la subcaracterística|  No|
|Servicios de impresión y fax|    No| No| Sí (Printbrm.exe)| No|
|Servicios de Escritorio remoto|   Sí, para todos los subroles, pero no se admite la granja de modo mixto|   Sí, para todos los subroles, pero no se admite la granja de modo mixto|   Sí|    No|
|Servidor web (IIS)|  Sí|    Sí|    Sí|    No|
|Experiencia con Windows Server Essentials|  Sí|    N/A: nueva característica|  Sí|    No|
|Windows Server Update Services|    Sí|    Sí|    Sí|    No|
|Carpetas de trabajo|  Sí|    Sí|    Sí|    Sí, en el clúster de WS 2012 R2, cuando se usa la [Actualización gradual de sistema operativo de clúster](https://technet.microsoft.com/windows-server-docs/failover-clustering/cluster-operating-system-rolling-upgrade).|

> [!IMPORTANT]  
> Una vez completada la instalación e inmediatamente después de instalar todos los roles de servidor y las características que necesita, busque e instale las actualizaciones disponibles para Windows Server 2016 mediante Windows Update u otros métodos de actualización.

---------------------------------------
Si necesita una opción de instalación diferente, o si ha terminado la instalación y está preparado para implementar cargas de trabajo específicas, puede ir [a la página principal de Windows Server 2016](Windows-Server-2016.md).