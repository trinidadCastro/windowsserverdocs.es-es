---
title: Instalación de Server Core
description: Cómo obtener e instalar una instalación Server Core en Windows Server 2019, Windows Server 2016 o Windows Server (canal semianual).
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.date: 1/04/2019
ms.technology: server-general
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 2d22818c-fbb7-487a-bb82-81ef0a3f7ede
author: jaimeo
ms.author: jaimeo
manager: dougkim
ms.localizationpriority: medium
ms.openlocfilehash: d99cd0b028d08d5c3247541ce3a868676b60693d
ms.sourcegitcommit: 7fc7271745e40f110c54918b55624cadd0d7ff98
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/04/2019
ms.locfileid: "8991802"
---
# Instalación de Server Core

> Se aplica a: Windows Server 2019, Windows Server 2016, Windows Server (canal semianual)
  
Cuando se instala Windows Server por primera vez, tienes las siguientes opciones de instalación:

>[!NOTE]
> En la lista siguiente, las ediciones sin la "Experiencia de escritorio" son las opciones de instalación Server Core

-   Windows Server Standard
-   Windows Server Standard con Experiencia de escritorio
-   Windows Server Datacenter
-   Windows Server Datacenter con Experiencia de escritorio

Cuando se instala Windows Server (canal semianual), incluidas las versiones 1709, 1803 y 1809, tienes las siguientes opciones de instalación:

-   Windows Server Standard 
-   Windows Server Datacenter

La opción Server Core reduce el espacio requerido en el disco y la posible superficie expuesta a ataques, de modo que recomendamos elegir la instalación Server Core a menos que necesites particularmente los elementos de la interfaz de usuario y las herramientas de administración de gráficos que se incluyen en la opción Servidor con Experiencia de escritorio. Si consideras que necesitas los elementos adicionales de la interfaz de usuario, consulta [Instalación de servidor con Experiencia de escritorio](Getting-Started-with-Server-with-Desktop-Experience.md). 

Con esta opción de Server Core, no se instala la interfaz de usuario estándar (la Experiencia de escritorio). El servidor se administra mediante la línea de comandos, mediante Windows PowerShell o mediante métodos remotos.

>[!NOTE]
>
>A diferencia de algunas versiones anteriores de Windows Server, no se puede convertir entre Server Core y Servidor con Experiencia de escritorio tras la instalación. Si instala Server Core y más tarde decides usar Servidor con Experiencia de escritorio, debes efectuar una instalación nueva.

**Interfaz de usuario:** Símbolo del sistema

**Instalar, configurar y desinstalar roles de servidor localmente:** en un símbolo del sistema con Windows PowerShell.

**Instalar, configurar, desinstalar roles de servidor de forma remota desde un equipo cliente de Windows (o instalar un servidor con experiencia de escritorio):** con el administrador del servidor, herramientas de administración remota de servidor (RSAT), Windows PowerShell o Windows Admin Center.

>[!NOTE]
>
>Para RSAT, debes usar la versión de Windows10.
>Microsoft Management Console no está disponible localmente.

**Roles de servidor de ejemplo disponibles:**

- Servicios de certificados de Active Directory
- Servicios de dominio de ActiveDirectory
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

Para las funciones no incluidas en Server Core, vea [Roles, servicios de rol y características no en Windows Server - Server Core](../administration/server-core/server-core-removed-roles.md).

## Instalar en Windows Server 2019 o Windows Server 2016

Para pasos generales de instalación y las opciones para Windows Server (canal mantenimiento a largo plazo), consulta la [actualización y la instalación de Windows Server](installation-and-upgrade.md).

## Instalar en Windows Server (canal semianual)

Pasos de instalación para Windows Server (canal semianual) son los mismos que para instalar versiones anteriores de Windows Server (desde un. Imagen ISO), con las siguientes excepciones:
- Las actualizaciones no admitidas de versiones anteriores de Windows Server para Windows Server, versión 1709. Una instalación nueva siempre es necesaria.
   Esto significa que cuando se ejecuta setup.exe desde el escritorio de un equipo de Windows, la experiencia de configuración no permite la opción de actualización (aparece atenuada).
- No hay ninguna versión de evaluación de Windows Server (canal semianual)
- No hay ninguna versión OEM o comercial. Windows Server (canal semianual) solo puede tener una licencia a través de programas de Software Assurance o fidelidad.

Para obtener Windows Server, versión 1709, consulta [Introducción a Windows Server, versión 1709](get-started-with-1709.md).

Para obtener Windows Server versión 1803, consulta la [Introducción a Windows Server, versión 1803](get-started-with-1803.md).

Para ver Novedades de Windows Server, versión 1809, consulte [What ' s New in Windows Server, versión 1809](whats-new-in-windows-server-1809.md)
