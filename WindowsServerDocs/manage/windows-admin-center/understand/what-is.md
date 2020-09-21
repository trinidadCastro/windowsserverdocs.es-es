---
title: ¿Qué es Windows Admin Center?
description: Qué es Windows Admin Center (Proyecto Honolulu)
ms.topic: article
author: nwashburn-ms
ms.author: niwashbu
ms.localizationpriority: medium
ms.date: 06/07/2019
ms.openlocfilehash: affbc610484abc5a4e45534a7f75e4f06efc23e9
ms.sourcegitcommit: 5344adcf9c0462561a4f9d47d80afc1d095a5b13
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/18/2020
ms.locfileid: "90766968"
---
# <a name="what-is-windows-admin-center"></a>¿Qué es Windows Admin Center?

> Se aplica a: Windows Admin Center, versión preliminar de Windows Admin Center

Windows Admin Center es un conjunto de herramientas de administración nuevo, implementado localmente y basado en explorador que permite administrar los servidores de Windows Server sin ninguna dependencia de Azure ni de la nube. Windows Admin Center ofrece el control total de todos los aspectos de tu infraestructura de servidores y es especialmente útil para la administración de servidores en redes privadas que no están conectadas a Internet.

Windows Admin Center es la evolución moderna de herramientas de administración como el Administrador de servidores y MMC. Complementa a System Center, no es su sustituto.

![Diagrama de Windows Admin Center funcionando con otras soluciones](../media/wac-complements.png)

## <a name="how-does-windows-admin-center-work"></a>¿Cómo funciona Windows Admin Center?

Windows Admin Center se ejecuta en un explorador web y administra Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012, Windows 10, Azure Stack HCI, etc. mediante la **puerta de enlace de Windows Admin Center** instalada en Windows Server o Windows 10 unido a un dominio. La puerta de enlace administra servidores mediante PowerShell remota y WMI a través de WinRM. La puerta de enlace se incluye con Windows Admin Center en un solo paquete .msi ligero que se puede [descargar](../overview.md).

La puerta de enlace de Windows Admin Center, cuando se publica en DNS y se le proporciona acceso a través de los firewalls corporativos correspondientes, no solo permite conectarse de forma segura a los servidores desde cualquier lugar con Microsoft Edge o Google Chrome, sino también administrarlos.

![Diagrama de la arquitectura de Windows Admin Center](../media/architecture.png)

## <a name="learn-how-windows-admin-center-improves-your-management-environment"></a>Obtén información acerca de la forma en que Windows Admin Center mejora tu entorno de administración

### <a name="familiar-functionality"></a>**Funcionalidad familiar**

Windows Admin Center es la evolución de plataformas de administración tradicionales conocidas como Microsoft Management Console (MMC), pero se ha creado desde un principio pensando en la forma en que los sistemas se crean y administran hoy en día. Windows Admin Center contiene muchas de las herramientas familiares que se utilizan actualmente para administrar clientes y servidores de Windows Server.

### <a name="easy-to-install-and-use"></a>**Fácil de instalar y usar**

[Instálalo](../deploy/install.md) en un equipo con Windows 10 y empieza a administrar en cuestión minutos, o bien instálalo en un servidor con Windows 2016 que actúe como puerta de enlace para posibilitar que toda la organización administre equipos desde su explorador web.

### <a name="complements-existing-solutions"></a>**Complementa las soluciones existentes**

Windows Admin Center funciona con soluciones como System Center y la administración y seguridad de Azure, y se agrega a sus funcionalidades para realizar tareas de administración detalladas en un solo equipo.

### <a name="manage-from-anywhere"></a>**Administración desde cualquier lugar**

Publica un servidor de puerta de enlace de Windows Admin Center en Internet pública y podrás conectarte y administrar los servidores desde cualquier lugar, yo todo de una forma segura.

### <a name="enhanced-security-for-your-management-platform"></a>**Seguridad mejorada para tu plataforma de administración**

Windows Admin Center tiene muchas mejoras que hacen que tu plataforma de administración sea [más segura](../plan/user-access-options.md). El control de acceso basado en roles permite ajustar con precisión qué administradores tienen acceso a cada característica de administración. Las opciones de autenticación de puerta de enlace incluyen grupos locales, Active Directory basado en dominios locales y Azure Active Directory basado en la nube.  También, puedes [obtener información útil](../use/logging.md) acerca de las acciones de administración realizadas en tu entorno.

### <a name="azure-integration"></a>**Integración de Azure**

Windows Admin Center tiene muchos puntos de [integración con servicios de Azure](../azure/index.md), incluidos Azure Active Directory, Azure Backup, Azure Site Recovery, etc.

### <a name="deploy-hyper-converged-and-failover-clusters"></a>**Implementación de clústeres hiperconvergidos y de conmutación por error**

Windows Admin Center permite una [implementación sin problemas de clústeres hiperconvergidos y de conmutación por error](../use/deploy-hyperconverged-infrastructure.md) a través de un asistente fácil de usar.

### <a name="manage-hyper-converged-clusters"></a>**Administración de clústeres hiperconvergidos**

Windows Admin Center ofrece la mejor experiencia para la [administración de clústeres hiperconvergidos](../use/manage-hyper-converged.md), lo que incluye el proceso, almacenamiento y componentes de red virtualizados.

### <a name="extensibility"></a>**Extensibilidad**

Windows Admin Center se creó desde el principio teniendo en mente la capacidad de ampliación, con la capacidad para que desarrolladores tanto de Microsoft como de terceros creen herramientas y soluciones que van más allá de las ofertas actuales. Microsoft ofrece un [SDK](../extend/extensibility-overview.md) que permite a los programadores crear sus propias herramientas para Windows Admin Center.

> [!Tip]
> ¿Estás listo para instalar Windows Admin Center? [Descargar ahora](../overview.md)