---
title: Novedades para cuentas de servicio administradas
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
ms.sourcegitcommit: 583355400f6b0d880dc0ac6bc06f0efb50d674f7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/17/2017
---
# <a name="what39s-new-for-managed-service-accounts"></a>¿Qué & #39; s de cuentas de servicio administradas

>Se aplica a: Windows Server (punto y anual canal), Windows Server 2016

En este tema para profesionales de TI describe los cambios en la funcionalidad de cuentas de servicio administradas con la introducción del grupo de cuenta de servicio administrada (gMSA) en Windows Server 2012 y Windows 8.

La cuenta de servicio administrada está diseñada para proporcionar servicios y tareas como el servicio de Windows y los grupos de aplicaciones de IIS para compartir sus propias cuentas de dominio, al eliminar la necesidad de un administrador administrar manualmente las contraseñas de estas cuentas. Es una cuenta de dominio administrado que proporciona administración automática de contraseñas.

## <a name="versions"></a>Novedades para cuentas de servicio administradas en Windows Server 2012 y Windows 8
A continuación describen los cambios en la funcionalidad se realizaron en MSA en Windows Server 2012 y Windows 8.

### <a name="group-managed-service-accounts"></a>Cuentas de servicio administrado de grupo
Cuando se configura una cuenta de dominio de un servidor en un dominio, el equipo cliente puede autenticar y conectarse a dicho servicio. Anteriormente, solo dos tipos de cuenta han proporcionado identidad sin necesidad de administración de contraseñas. Pero estos tipos de cuenta tienen limitaciones:

-   Cuenta de equipo está limitada a un servidor de dominio y las contraseñas son administradas por el equipo

-   Cuenta de servicio administrada está limitada a un servidor de dominio y las contraseñas son administradas por el equipo.

Estas cuentas no se pueden compartir a través de varios sistemas. Por lo tanto, debes mantener la cuenta para cada servicio en cada sistema para evitar la expiración de contraseña no deseados con regularidad.

**¿Qué valor agregar este cambio?**

La cuenta de servicio administrado de grupo resuelve este problema porque la contraseña de cuenta se administra mediante controladores de dominio de Windows Server 2012 y se puede recuperar con varios sistemas Windows Server 2012. Esto reduce la sobrecarga administrativa de una cuenta de servicio al permitir que Windows controlar la administración de contraseñas de estas cuentas.

**¿Qué funciona de manera diferente?**

En equipos que ejecutan Windows Server 2012 o Windows 8, un grupo de MSA puede crearse y se administran mediante el Administrador de Control de servicios para que varias instancias del servicio, como implementan a través de una granja de servidores, puede administrar desde un servidor. Herramientas y utilidades que usaste para administrar cuentas de servicio administradas, como el Administrador de grupo de aplicaciones de IIS, pueden usarse con cuentas de servicio administradas de grupo. Los administradores de dominio pueden delegar la administración de servicios a los administradores de servicio, que pueden administrar todo el ciclo de vida de una cuenta de servicio administrada o la cuenta de servicio administrado de grupo. Los equipos cliente existentes podrán autenticarse en cualquier servicio sin saber qué instancia del servicio se autentican en.

### <a name="interoperability"></a>Funcionalidad desusada o eliminada
Para Windows Server 2012, el valor predeterminado de cmdlets de Windows PowerShell para administrar las cuentas de servicio administradas en grupo en lugar de las cuentas de servicio de servidor administrado.

## <a name="see-also"></a>Consulta también

-   [Introducción a las cuentas servicio administrado grupo](group-managed-service-accounts-overview.md)

-   [Información general de servicios de dominio de Active Directory](active-directory-domain-services-overview.md)

-   [Descripción de las cuentas de servicio administradas:, Implementar, procedimientos recomendados y solución de problemas](http://blogs.technet.com/b/askds/archive/20../managed-service-accounts-understanding-implementing-best-practices-and-troubleshooting.aspx)


