---
title: Instalación de Server Core
description: Cómo obtener e instalar una instalación Server Core en Windows Server 2019, Windows Server 2016 o Windows Server (canal semianual).
ms.prod: windows-server-threshold
ms.date: 05/21/2019
ms.technology: server-general
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 2d22818c-fbb7-487a-bb82-81ef0a3f7ede
author: jasongerend
ms.author: jgerend
manager: dougkim
ms.localizationpriority: medium
ms.openlocfilehash: 6f685ce29088b56bb243d21315787ab90e6863a4
ms.sourcegitcommit: c8cc0b25ba336a2aafaabc92b19fe8faa56be32b
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/21/2019
ms.locfileid: "65976721"
---
# <a name="install-server-core"></a>Instalación de Server Core

> Se aplica a: Windows Server 2019, Windows Server 2016, Windows Server (canal semianual)
  
Cuando se instala Windows Server por primera vez, tiene las siguientes opciones de instalación:

>[!NOTE]
> En la lista siguiente, las ediciones sin la "Experiencia de escritorio" son las opciones de instalación Server Core

-   Windows Server Standard
-   Windows Server Standard con Experiencia de escritorio
-   Windows Server Datacenter
-   Windows Server Datacenter con Experiencia de escritorio

Cuando se instala Windows Server (canal semianual), tiene las siguientes opciones de instalación:

-   Windows Server Standard 
-   Windows Server Datacenter

La opción Server Core reduce el espacio requerido en el disco y la posible superficie expuesta a ataques, de modo que recomendamos elegir la instalación Server Core a menos que necesites particularmente los elementos de la interfaz de usuario y las herramientas de administración de gráficos que se incluyen en la opción Servidor con Experiencia de escritorio. Si consideras que necesitas los elementos adicionales de la interfaz de usuario, consulta [Instalación de servidor con Experiencia de escritorio](Getting-Started-with-Server-with-Desktop-Experience.md). 

Con esta opción de Server Core, no se instala la interfaz de usuario estándar (la Experiencia de escritorio). El servidor se administra mediante la línea de comandos, mediante Windows PowerShell o mediante métodos remotos.

>[!NOTE]
>
>A diferencia de algunas versiones anteriores de Windows Server, no se puede convertir entre Server Core y Servidor con Experiencia de escritorio tras la instalación. Si instala Server Core y más tarde decides usar Servidor con Experiencia de escritorio, debes efectuar una instalación nueva.

**Interfaz de usuario:** símbolo del sistema

**Instalar, configurar, desinstalar roles de servidor localmente:** en un símbolo del sistema con Windows PowerShell.

**Instalar, configurar, desinstalar roles de servidor de forma remota desde un equipo cliente de Windows (o un servidor con la experiencia de escritorio instalado):** con el administrador del servidor, herramientas de administración remota de servidor (RSAT), Windows PowerShell o Windows Admin Center .

>[!NOTE]
>
>Para RSAT, debe usar la versión de Windows 10.
>Microsoft Management Console no está disponible localmente.

**Roles de servidor de ejemplo disponibles:**

- Servicios de certificados de Active Directory
- Active Directory Domain Services
- Servidor DHCP
- Servidor DNS
- Servicios de archivo (incluido Administrador de recursos del servidor de archivos)
- Active Directory Lightweight Directory Services (AD LDS)
- Hyper-V
- Servicios de impresión y documentos
- Servicios de multimedia de transmisión por secuencias
- Servidor web (incluido un subconjunto de ASP.NET)
- Servidor Windows Server Update
- Servidor de Active Directory Rights Management
- Enrutamiento y acceso remoto y los siguientes subroles:
   - Agente de conexión a Servicios de Escritorio remoto
   - Concesión de licencias
   - Virtualización
   - Volume Activation Services

Para los roles no incluidas en Server Core, vea [Roles, servicios de rol y características no incluidas en Windows Server - Server Core](../administration/server-core/server-core-removed-roles.md).

## <a name="installing-on-windows-server-2019-or-windows-server-2016"></a>Instalación en Windows Server 2019 o Windows Server 2016

Para los pasos de instalación general y opciones de Windows Server (canal de servicio de largo plazo), consulte [Windows Server Installation and Upgrade](installation-and-upgrade.md).

## <a name="installing-on-windows-server-semi-annual-channel"></a>Instalación en Windows Server (canal semianual)

Pasos de instalación para Windows Server (canal semianual) son los mismos que la instalación de las versiones anteriores de Windows Server (desde una. Imagen ISO), con las siguientes excepciones:

- Las actualizaciones no admitidas de versiones anteriores de Windows Server para Windows Server, versión 1709. Una instalación nueva siempre es necesaria.
   Esto significa que cuando ejecute setup.exe desde el escritorio de un equipo Windows, la experiencia de instalación no permite la opción de actualización (está atenuado).
- No hay ninguna versión de evaluación para Windows Server (canal semianual)
- No hay ninguna versión OEM o comercial. Windows Server (canal semianual) solo puede tener una licencia a través de programas de Software Assurance o fidelidad.

Para obtener más información acerca del canal semianual, consulte [comparación de canales de mantenimiento](../get-started-19/servicing-channels-19.md).

Para ver cuáles son las novedades en canal semianual de Windows Server, consulte [What ' s New in Windows Server](whats-new-in-windows-server.md)
