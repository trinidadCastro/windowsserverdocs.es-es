---
title: Instalación de Server Core
description: Cómo obtener una instalación Server Core e instalarla en Windows Server 2019, Windows Server 2016 o Windows Server (canal semianual).
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
ms.sourcegitcommit: 3743cf691a984e1d140a04d50924a3a0a19c3e5c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/17/2019
ms.locfileid: "65976721"
---
# <a name="install-server-core"></a>Instalación de Server Core

> Se aplica a: Windows Server 2019, Windows Server 2016, Windows Server (canal semianual)
  
Cuando instalas Windows Server por primera vez, tienes las siguientes opciones de instalación:

>[!NOTE]
> En la lista siguiente, las ediciones sin la "Experiencia de escritorio" son las opciones de instalación básica.

-   Windows Server Standard
-   Windows Server Standard con Experiencia de escritorio
-   Windows Server Datacenter
-   Windows Server Datacenter con Experiencia de escritorio

Cuando instalas Windows Server (canal semianual), tienes las siguientes opciones de instalación:

-   Windows Server Standard 
-   Windows Server Datacenter

La opción de instalación básica reduce el espacio requerido en el disco y la posible superficie expuesta a ataques, de modo que recomendamos elegir la instalación básica, a menos que necesites particularmente los elementos adicionales de la interfaz de usuario y las herramientas de administración de gráficos que se incluyen en la opción de Server con Experiencia de escritorio. Si consideras que necesitas los elementos adicionales de la interfaz de usuario, consulta [Instalación de servidor con Experiencia de escritorio](Getting-Started-with-Server-with-Desktop-Experience.md). 

Con esta opción de instalación básica, no se instala la interfaz de usuario estándar (la Experiencia de escritorio). El servidor se administra mediante la línea de comandos, Windows PowerShell o métodos remotos.

>[!NOTE]
>
>A diferencia de algunas versiones anteriores de Windows Server, no se puede convertir entre Server Core y Servidor con Experiencia de escritorio tras la instalación. Si instala Server Core y más tarde decides usar Servidor con Experiencia de escritorio, debes efectuar una instalación nueva.

**Interfaz de usuario:** símbolo del sistema

**Instalar, configurar, desinstalar roles de servidor localmente:** en un símbolo del sistema con Windows PowerShell.

**Instalar, configurar, desinstalar roles de servidor remotamente desde un equipo cliente de Windows (o servidor con Experiencia de escritorio instalada):** con el Administrador de servidores, Herramientas de administración remota del servidor (RSAT), Windows PowerShell o Windows Admin Center.

>[!NOTE]
>
>Para RSAT, debe usar la versión de Windows 10.
>Microsoft Management Console no está disponible localmente.

**Ejemplo de roles de servidor disponibles:**

- Servicios de certificados de Active Directory
- Active Directory Domain Services
- Servidor DHCP
- Servidor DNS
- Servicios de archivo (incluido Administrador de recursos del servidor de archivos)
- Active Directory Lightweight Directory Services (AD LDS)
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

Para los roles no incluidos en la instalación básica, consulta [Roles, servicios de rol y características no incluidas en Windows Server - Server Core](../administration/server-core/server-core-removed-roles.md).

## <a name="installing-on-windows-server-2019-or-windows-server-2016"></a>Instalación en Windows Server 2019 o Windows Server 2016

Por pasos generales de instalación y opciones para Windows Server (canal de servicio a largo plazo), consulta [Instalación y actualización de Windows Server](installation-and-upgrade.md).

## <a name="installing-on-windows-server-semi-annual-channel"></a>Instalación en Windows Server (canal semianual)

Los pasos de instalación para Windows Server (canal semianual) son los mismos que para instalar versiones anteriores de Windows Server (de una imagen .ISO), con las siguientes excepciones:

- No se admiten actualizaciones desde versiones anteriores de Windows Server hasta Windows Server, versión 1709. Siempre es necesaria una instalación nueva.
   Esto significa que cuando se ejecuta setup.exe desde el escritorio de un equipo Windows, la experiencia de configuración no permite la opción de actualización (aparece atenuada).
- No hay ninguna versión de evaluación de Windows Server (canal semianual).
- No hay ninguna versión de OEM ni comercial. Se pueden obtener licencias de Windows Server (canal semianual) a través de Software Assurance o los programas de fidelidad.

Para más información sobre el canal semianual, consulta [Comparación de los canales de mantenimiento](../get-started-19/servicing-channels-19.md).

Para ver las novedades del canal semianual de Windows Server, consulta [Novedades de Windows Server](whats-new-in-windows-server.md).
