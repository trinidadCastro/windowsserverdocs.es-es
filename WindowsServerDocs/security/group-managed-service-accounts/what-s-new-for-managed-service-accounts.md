---
title: What's New for Managed Service Accounts
description: Seguridad de Windows Server
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: security-gmsa
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 2f2a8b6b-c152-4c40-b712-bfabff0e408b
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/12/2016
ms.openlocfilehash: 82d0ce962dfab0f7c9e5180e4b471894f507fb26
ms.sourcegitcommit: 083ff9bed4867604dfe1cb42914550da05093d25
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/14/2020
ms.locfileid: "75950345"
---
# <a name="what39s-new-for-managed-service-accounts"></a>Novedades&#39;de las cuentas de servicio administradas

>Se aplica a: Windows Server (canal semianual), Windows Server 2016

En este tema para profesionales de TI se describen los cambios en la funcionalidad de las cuentas de servicio administradas con la introducción de la cuenta de servicio administrada de grupo (gMSA) en Windows Server 2012 y Windows 8.

La cuenta de servicio administrada se ha diseñado para proporcionar servicios y tareas como los servicios de Windows y los grupos de aplicaciones de IIS a fin de compartir sus propias cuentas de dominio, a la vez que se elimina la necesidad de que un administrador administre manualmente las contraseñas de estas cuentas. Se trata de una cuenta de dominio administrada que ofrece administración de contraseñas automática.

## <a name="versions"></a>Novedades de las cuentas de servicio administradas en Windows Server 2012 y Windows 8
A continuación se describen los cambios en la funcionalidad que se realizaron en MSA en Windows Server 2012 y Windows 8.

### <a name="group-managed-service-accounts"></a>Group Managed Service Accounts
Cuando una cuenta de dominio se configura para un servidor de dominio, el equipo cliente puede autenticarse y conectarse a ese servicio. Antes, solo dos tipos de cuenta ofrecían identidad sin requerir administración de contraseñas. Sin embargo, estos tipos de cuenta tienen limitaciones:

-   La cuenta de equipo se limita a un servidor de dominio y el equipo administra las contraseñas.

-   La cuenta de servicio administrada se limita a un servidor de dominio y el equipo administra las contraseñas.

Estas cuentas no se pueden compartir entre varios sistemas. Por lo tanto, periódicamente se debe realizar un mantenimiento de la cuenta para cada servicio de cada sistema a fin de prevenir la expiración no deseada de las contraseñas.

**¿Qué valor aporta este cambio?**

La cuenta de servicio administrada de Grupo resuelve este problema porque la contraseña de la cuenta está administrada por controladores de dominio de Windows Server 2012 y puede ser recuperada por varios sistemas de Windows Server 2012. Esto minimiza la sobrecarga administrativa de una cuenta de servicio, ya que permite que Windows controle la administración de contraseñas para estas cuentas.

**¿Qué funciona de manera diferente?**

En los equipos que ejecutan Windows Server 2012 o Windows 8, se puede crear una MSA de grupo y administrarla mediante el administrador de control de servicios para que se puedan administrar varias instancias del servicio, como se implementan en una granja de servidores, desde un servidor. Las herramientas y utilidades que se usan para administrar las cuentas de servicio administradas, tales como el Administrador de grupos de aplicaciones de IIS, se pueden utilizar con cuentas de servicio administradas por grupo. Los administradores de dominios pueden delegar la administración de servicios a los administradores de servicios, que pueden administrar todo el ciclo de vida de una cuenta de servicio administrada o una cuenta de servicio administrada por grupo. Los equipos cliente actuales podrán autenticarse con cualquier servicio sin saber con qué instancia de servicio se autentican.

### <a name="interoperability"></a>Funcionalidad eliminada o desusada
En Windows Server 2012, los cmdlets de Windows PowerShell predeterminados para administrar las cuentas de servicio administradas de grupo en lugar de las cuentas de servicio administradas del servidor.

## <a name="see-also"></a>Consulta también

-   [Información general de las cuentas de servicio administradas de grupo](group-managed-service-accounts-overview.md)

-   [Introducción a Active Directory Domain Services](active-directory-domain-services-overview.md)

-   [Cuentas de servicio administradas: Descripción, implementación, procedimientos recomendados y solución de problemas](https://blogs.technet.com/b/askds/archive/20../managed-service-accounts-understanding-implementing-best-practices-and-troubleshooting.aspx)


