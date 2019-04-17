---
title: Qué es Windows Admin Center
description: Qué es Windows Admin Center (Proyecto Honolulu)
ms.technology: manage
ms.topic: article
author: nwashburn-ms
ms.author: niwashbu
ms.date: 06/18/2018
ms.localizationpriority: medium
ms.prod: windows-server-threshold
ms.openlocfilehash: d374704768d06aaeb7ee6bc9c984cc78d49cb4b0
ms.sourcegitcommit: be0144eb59daf3269bebea93cb1c467d67e2d2f1
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/20/2018
ms.locfileid: "4080912"
---
# ¿Qué es Windows Admin Center?

>Se aplica a: Windows Admin Center, Versión preliminar de Windows Admin Center

Windows Admin Center es un conjunto de herramientas de administración nuevo, implementado localmente, basado en el explorador que le permite administrar los servidores de Windows sin ninguna dependencia de Azure o la nube. Windows Admin Center le ofrece el control total de todos los aspectos de tu infraestructura de servidores y es especialmente útil para la administración de servidores en las redes privadas que no están conectados a Internet.

Windows Admin Center es la evolución moderna de herramientas de administración incluidas, como el Administrador de servidores y MMC. Complementa a System Center: no es un sustituto.

![](../media/wac-complements.png)

## ¿Cómo funciona Windows Admin Center?

Windows Admin Center se ejecuta en un explorador web y administra Windows Server 2016, Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2, Windows 10 y más información a través de la **puerta de enlace de Windows Admin Center** instalada en Windows Server 2016 o Windows 10. La puerta de enlace administra servidores utilizando PowerShell remota y WMI a través de WinRM. La puerta de enlace se incluye con Windows Admin Center en un solo paquete .msi ligero que puedes [descargar](https://aka.ms/windowsadmincenter).

La puerta de enlace de Windows Admin Center, cuando se publica en DNS y se je proporciona acceso a través de los firewalls corporativos correspondientes, permite conectarse de forma segura a y administrar los servidores desde cualquier lugar con Microsoft Edge o Google Chrome.

![](../media/architecture.png)

## Obtén información sobre cómo Windows Admin Center mejora tu entorno de administración

### **Funcionalidad familiar**

Windows Admin Center es la evolución de plataformas de administración tradicionales conocidas como Microsoft Management Console (MMC), integrada desde el principio para la forma en que los sistemas se crean y administran hoy en día. Windows Admin Center contiene muchas de las herramientas familiares que se utilizan actualmente para administrar los clientes y servidores de Windows.

### **Fácil de instalar y usar**

[Instala](../deploy/install.md) en un equipo Windows 10 y empieza a administrar en minutos, o instala en un servidor de Windows 2016 que actúe como una puerta de enlace para posibilitar que toda la organización administre equipos desde su navegador web.

### **Complementa las soluciones existentes** 

Windows Admin Center funciona con soluciones como System Center y Azure la administración y la seguridad, agregando a sus capacidades para realizar detallada, las tareas de administración solo equipo.

### **Administrar desde cualquier lugar**

Publica el servidor de puerta de enlace de Windows Admin Center en Internet pública; a continuación, podrás conectar a y administrar los servidores desde cualquier lugar, todo de un modo seguro.

### **Seguridad mejorada para tu plataforma de administración**

Windows Admin Center tiene muchas mejoras que hacen que tu plataforma de administración sea [más segura](../plan/user-access-options.md). El control de acceso basado en roles permite ajustar con precisión qué administradores tienen acceso a qué características de administración. Las opciones de autenticación de puerta de enlace incluyen grupos locales, Active Directory basado en dominio local y Azure Active Directory basado en la nube.  También, [obtén información valiosa](../use/logging.md) sobre las acciones de administración realizadas en su entorno.

### **Integración de Azure**

Windows Admin Center tiene muchos puntos de [integración con servicios de Azure](../plan/azure-integration-options.md), incluidos Azure Active Directory, Azure Backup, Azure Site Recovery y mucho más.

### **Administrar clústeres hiperconvergidos**

Windows Admin Center ofrece la mejor experiencia para la [administración de clústeres hiperconvergidos](../use/manage-hyper-converged.md), incluida computación virtualizada, almacenamiento y componentes de red.

### **Extensibilidad**

Windows Admin Center se ha creadp teniendo en mente la capacidad de ampliación desde el principio, con la capacidad de desarrolladores de Microsoft y externos para crear herramientas y soluciones más allá de las ofertas actuales. Microsoft ofrece un [SDK](../extend/extensibility-overview.md) que permite a los programadores crear sus propias herramientas para Windows Admin Center.

> [!Tip]
> ¿Estás listo para instalar Windows Admin Center? [Descargar ahora](https://aka.ms/windowsadmincenter)
