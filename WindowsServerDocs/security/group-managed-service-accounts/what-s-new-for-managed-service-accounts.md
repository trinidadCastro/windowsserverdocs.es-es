---
title: What's New for Managed Service Accounts
description: Seguridad de Windows Server
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: cac55d04a40c84ce160eb3883d6095a7db0ef3be
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59872186"
---
# <a name="what39s-new-for-managed-service-accounts"></a>¿Qué&#39;s nuevas cuentas de servicio administradas

>Se aplica a: Windows Server (canal semianual), Windows Server 2016

En este tema para profesionales de TI describe los cambios en la funcionalidad de las cuentas de servicio administradas con la introducción del grupo de cuenta de servicio administrada (gMSA) en Windows Server 2012 y Windows 8.

La cuenta de servicio administrada se ha diseñado para proporcionar servicios y tareas como los servicios de Windows y los grupos de aplicaciones de IIS a fin de compartir sus propias cuentas de dominio, a la vez que se elimina la necesidad de que un administrador administre manualmente las contraseñas de estas cuentas. Se trata de una cuenta de dominio administrada que ofrece administración de contraseñas automática.

## <a name="versions"></a>Novedades de las cuentas de servicio administradas en Windows Server 2012 y Windows 8
A continuación describen los cambios de funcionalidad que se aplicaron a MSA en Windows Server 2012 y Windows 8.

### <a name="group-managed-service-accounts"></a>Cuentas de servicio administradas de grupo
Cuando una cuenta de dominio se configura para un servidor de dominio, el equipo cliente puede autenticarse y conectarse a ese servicio. Antes, solo dos tipos de cuenta ofrecían identidad sin requerir administración de contraseñas. Sin embargo, estos tipos de cuenta tienen limitaciones:

-   La cuenta de equipo se limita a un servidor de dominio y el equipo administra las contraseñas.

-   La cuenta de servicio administrada se limita a un servidor de dominio y el equipo administra las contraseñas.

Estas cuentas no se pueden compartir entre varios sistemas. Por lo tanto, periódicamente se debe realizar un mantenimiento de la cuenta para cada servicio de cada sistema a fin de prevenir la expiración no deseada de las contraseñas.

**¿Qué valor aporta este cambio?**

La cuenta de servicio administrada por grupo soluciona este problema, ya que la contraseña de cuenta se administra mediante los controladores de dominio de Windows Server 2012 y se puede recuperar varios sistemas de Windows Server 2012. Esto minimiza la sobrecarga administrativa de una cuenta de servicio, ya que permite que Windows controle la administración de contraseñas para estas cuentas.

**¿Qué funciona de manera diferente?**

En los equipos que ejecutan Windows Server 2012 o Windows 8, un grupo de MSA puede crearse y administrarse mediante el Administrador de Control de servicios para que varias instancias del servicio, como se implementan a través de una granja de servidores, se puede administrar desde un servidor. Las herramientas y utilidades que se usan para administrar las cuentas de servicio administradas, tales como el Administrador de grupos de aplicaciones de IIS, se pueden utilizar con cuentas de servicio administradas por grupo. Los administradores de dominios pueden delegar la administración de servicios a los administradores de servicios, que pueden administrar todo el ciclo de vida de una cuenta de servicio administrada o una cuenta de servicio administrada por grupo. Los equipos cliente actuales podrán autenticarse con cualquier servicio sin saber con qué instancia de servicio se autentican.

### <a name="interoperability"></a>Funcionalidad desusada o eliminada
Para Windows Server 2012, el valor predeterminado de los cmdlets de Windows PowerShell para administrar las cuentas de servicio administradas en grupo en lugar de las cuentas de servicio administradas de servidor.

## <a name="see-also"></a>Vea también

-   [Introducción a las cuentas de servicio administradas de grupo](group-managed-service-accounts-overview.md)

-   [Introducción a servicios de dominio de Active Directory](active-directory-domain-services-overview.md)

-   [Cuentas de servicio administradas: Descripción, implementación, procedimientos recomendados y solución de problemas](http://blogs.technet.com/b/askds/archive/20../managed-service-accounts-understanding-implementing-best-practices-and-troubleshooting.aspx)


