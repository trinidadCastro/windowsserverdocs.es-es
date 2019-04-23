---
ms.assetid: ac6604b0-7459-4ff3-af1c-4936897f5d14
title: Delegar la administración de unidades organizativas y contenedores predeterminados
description: ''
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 9d482854fd82b4bf0d0e61315d36e6222470ca55
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59830896"
---
# <a name="delegating-administration-of-default-containers-and-ous"></a>Delegar la administración de unidades organizativas y contenedores predeterminados

>Se aplica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Cada dominio de Active Directory contiene un conjunto estándar de los contenedores y las unidades organizativas (OU) que se crean durante la instalación de servicios de dominio de Active Directory (AD DS). A continuación se enumeran algunas:  
  
-   Contenedor de dominio, que actúa como el contenedor raíz a la jerarquía  
  
-   Contenedor integrado, que contiene el valor predeterminado de las cuentas de administrador de servicio  
  
-   Contenedor de usuarios, que es la ubicación predeterminada de nuevas cuentas de usuario y grupos se crea en el dominio  
  
-   Contenedor de equipos, que es la ubicación predeterminada para las nuevas cuentas de equipo creados en el dominio  
  
-   Unidad organizativa controladores de dominio, que es la ubicación predeterminada para las cuentas de equipo de cuentas de equipo de controladores de dominio  
  
El propietario del bosque controla estas unidades organizativas y contenedores predeterminados.  
  
## <a name="domain-container"></a>Contenedor de dominio  
El contenedor de dominio es el contenedor raíz de la jerarquía de un dominio. Los cambios realizados en las directivas o la lista de control de acceso (ACL) en este contenedor pueden tener impacto en todo el dominio. No se delegue el control de este contenedor. los administradores de servicios debe controlar.  
  
## <a name="users-and-computers-containers"></a>Usuarios y equipos de contenedores  
Al realizar una actualización local de dominios de Windows Server 2003 a Windows Server 2008, los usuarios y equipos existentes se colocan automáticamente en los usuarios y los contenedores de equipos. Si va a crear un nuevos contenedores de dominio, los usuarios y equipos de Active Directory son las ubicaciones predeterminadas para todas las nuevas cuentas de usuario y cuentas de equipo de controlador de dominio en el dominio.  
  
> [!IMPORTANT]  
> Si desea delegar el control de usuarios o equipos, no modifique la configuración predeterminada en los usuarios y equipos de contenedores. En su lugar, crear nuevas unidades organizativas (según sea necesario) y mover los objetos de usuario y equipo desde sus contenedores predeterminados y en las nuevas unidades organizativas. Delegar el control sobre las nuevas unidades organizativas, según sea necesario. Le recomendamos que no modifique que controla los contenedores predeterminados.  
  
Además, no se puede aplicar la configuración de directiva de grupo a los equipos y usuarios de forma predeterminada los contenedores. Para aplicar la directiva de grupo a usuarios y equipos, crear nuevas unidades organizativas y mover los objetos de usuario y equipo en esas unidades organizativas. Aplicar la configuración de directiva de grupo a las nuevas unidades organizativas.  
  
Si lo desea, puede redirigir la creación de objetos que se colocan en los contenedores predeterminados que se colocarán en contenedores de su elección.  
  
## <a name="well-known-users-and-groups-and-built-in-accounts"></a>Well-Known a los usuarios, grupos y cuentas integradas  
De forma predeterminada, se crean varios conocidos a los usuarios, grupos y cuentas integradas en un nuevo dominio. Se recomienda que la administración de estas cuentas permanece bajo el control de los administradores de servicios. No se delegar la administración de estas cuentas a una persona que no sea un administrador de servicios. En la tabla siguiente se enumera los conocidos a los usuarios y grupos y cuentas integradas que deben permanecer bajo el control de los administradores de servicios.  
  
|Well-Known usuarios y grupos|Cuentas integradas|  
|--------------------------------|----------------------|  
|Publicadores de certificados<br /><br />Controladores de dominio<br /><br />Propietarios del creador de directivas de grupo<br /><br />KRBTGT<br /><br />Invitados del dominio<br /><br />Administrador<br /><br />Admins. del dominio<br /><br />Administradores de esquema (solo dominio raíz del bosque)<br /><br />Administradores de organización (solo dominio raíz del bosque)<br /><br />Usuarios del dominio|Administrador<br /><br />Invitado<br /><br />Invitados<br /><br />Operadores de cuentas<br /><br />Administradores<br /><br />Operadores de copia de seguridad<br /><br />Creadores de confianza de bosque de entrada<br /><br />Opers. de impresión<br /><br />Acceso compatible con versiones anteriores de Windows 2000<br /><br />Operadores de servidores<br /><br />Usuarios|  
  
## <a name="domain-controller-ou"></a>Unidad organizativa del controlador de dominio  
Cuando se agregan controladores de dominio al dominio, sus objetos de equipo se agregan automáticamente a la OU de controlador de dominio. Esta unidad organizativa tiene un conjunto predeterminado de las directivas aplicadas a él. Para asegurarse de que estas directivas se aplican uniformemente a todos los controladores de dominio, se recomienda que no mueva los objetos de equipo los controladores de dominio de esta OU. Si se aplican las directivas predeterminadas puede producir un controlador de dominio no funcionará correctamente.  
  
De forma predeterminada, los administradores de servicios controlan esta unidad organizativa. No se delegar el control de esta unidad organizativa a personas que no sean los administradores de servicios.  
  


