---
ms.assetid: ac6604b0-7459-4ff3-af1c-4936897f5d14
title: "Delegar la administración de unidades organizativas y contenedores predeterminados"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 2504208210c03193451d19478f3bc8c98ec98f23
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/03/2017
---
# <a name="delegating-administration-of-default-containers-and-ous"></a>Delegar la administración de unidades organizativas y contenedores predeterminados

>Se aplica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Cada dominio de Active Directory contiene un conjunto estándar de contenedores y unidades organizativas (UO) que se crean durante la instalación de los servicios de dominio de Active Directory (AD DS). Estas son las siguientes acciones:  
  
-   Contenedor de dominio, que actúa como el contenedor raíz para la jerarquía  
  
-   Contenedor integrada, que contiene el valor predeterminado de las cuentas de administrador de servicio  
  
-   Contenedor de los usuarios, que es la ubicación predeterminada para las nuevas cuentas de usuario y los grupos creados en el dominio  
  
-   Contenedor de equipos, que es la ubicación predeterminada para las nuevas cuentas de equipo creado en el dominio  
  
-   Controladores de dominio, que es la ubicación predeterminada para las cuentas de equipo para cuentas de equipo de controladores de dominio  
  
El propietario de bosque controla estos contenedores predeterminados y unidades organizativas.  
  
## <a name="domain-container"></a>Contenedor de dominio  
El contenedor de dominio es el contenedor de la raíz de la jerarquía de un dominio. Cambios en las directivas o en la lista de control de acceso (ACL) en este contenedor pueden tener impacto en todo el dominio. No se delega el control de este contenedor. se deben ser controlada por los administradores de servicio.  
  
## <a name="users-and-computers-containers"></a>Los usuarios y equipos contenedores  
Al realizar una actualización en contexto dominio de Windows Server 2003 a Windows Server 2008, usuarios y equipos existentes se colocan automáticamente en los usuarios y los contenedores de equipos. Si vas a crear un nuevo contenedores de dominio, los usuarios y equipos de Active Directory son las ubicaciones predeterminadas para todas las nuevas cuentas de usuario y cuentas de equipo de controlador de dominio no del dominio.  
  
> [!IMPORTANT]  
> Si necesitas delegar el control de usuarios o equipos, no modificar la configuración predeterminada de los usuarios y equipos contenedores. En su lugar, crear nuevas unidades organizativas (según sea necesario) y mover los objetos de usuario y del equipo de sus contenedores predeterminado y en las nuevas unidades organizativas. Delegar control sobre las nuevas unidades organizativas según sea necesario. Te recomendamos que no modificar quién controla los contenedores de forma predeterminada.  
  
Además, no se puede aplicar la configuración de directiva de grupo para los usuarios de forma predeterminada y equipos contenedores. Para aplicar la directiva de grupo a los usuarios y equipos, crear nuevas unidades organizativas y mover los objetos de usuario y del equipo en dichas unidades organizativas. Aplicar la configuración de directiva de grupo a las nuevas unidades organizativas.  
  
Opcionalmente, puedes redirigir la creación de objetos que se colocan en los contenedores predeterminados debe colocarse en contenedores de tu elección.  
  
## <a name="well-known-users-and-groups-and-built-in-accounts"></a>Los usuarios conocidos, grupos y cuentas integradas  
De manera predeterminada, se crean varias conocidas a los usuarios, grupos y cuentas integradas de un dominio nuevo. Te recomendamos que administrar estas cuentas permanece bajo el control de los administradores de servicio. Administrar estas cuentas no se delega a una persona que no sea un administrador de servicios. La siguiente tabla enumera los conocidas a los usuarios y grupos y cuentas integradas que necesitan permanecer en el control de los administradores de servicio.  
  
|Grupos y usuarios conocidos|Cuentas integradas|  
|--------------------------------|----------------------|  
|Publicadores de certificados<br /><br />Controladores de dominio<br /><br />Propietarios del creador de directivas de grupo<br /><br />KRBTGT<br /><br />Invitados de dominio<br /><br />Administrador<br /><br />Administradores de dominio<br /><br />Administradores de esquema (solo dominio raíz del bosque)<br /><br />Administradores de empresa (solo dominio raíz del bosque)<br /><br />Usuarios del dominio|Administrador<br /><br />Invitado<br /><br />Invitados<br /><br />Operadores de cuentas<br /><br />Administradores<br /><br />Operadores de copia de seguridad<br /><br />Creadores de confianza de bosque de entrada<br /><br />Operadores de impresión<br /><br />Acceso de compatible con versiones anteriores de Windows 2000<br /><br />Operadores de servidor<br /><br />Usuarios|  
  
## <a name="domain-controller-ou"></a>Unidad organizativa del controlador de dominio  
Cuando se agregan controladores de dominio al dominio, los objetos del equipo se agregan automáticamente a la unidad organizativa de controlador de dominio. Esta unidad organizativa tiene un conjunto predeterminado de las directivas aplicadas a ella. Para garantizar que estas directivas se aplican uniformemente a todos los controladores de dominio, te recomendamos que no se mueve los objetos de equipo los controladores de dominio de esta OU. Si no se puede aplicar las directivas predeterminadas puede causar un controlador de dominio no funcionará correctamente.  
  
De manera predeterminada, los administradores de servicio controlan esta unidad organizativa. No se delega el control de esta unidad organizativa a las personas que no sean los administradores de servicio.  
  


