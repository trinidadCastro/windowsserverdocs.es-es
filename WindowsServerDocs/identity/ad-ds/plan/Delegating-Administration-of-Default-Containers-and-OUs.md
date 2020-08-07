---
ms.assetid: ac6604b0-7459-4ff3-af1c-4936897f5d14
title: Delegar la administración de unidades organizativas y contenedores predeterminados
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.openlocfilehash: 1bce2ce312b3105d3c347da1e491601bf15364e7
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/07/2020
ms.locfileid: "87947693"
---
# <a name="delegating-administration-of-default-containers-and-ous"></a>Delegar la administración de unidades organizativas y contenedores predeterminados

>Se aplica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Cada dominio de Active Directory contiene un conjunto estándar de contenedores y unidades organizativas (OU) que se crean durante la instalación de Active Directory Domain Services (AD DS). Entre ellas, figuran:

-   Contenedor de dominio, que actúa como contenedor raíz en la jerarquía

-   Contenedor integrado, que contiene las cuentas de administrador de servicios predeterminadas

-   Contenedor de usuarios, que es la ubicación predeterminada para las nuevas cuentas de usuario y los grupos creados en el dominio

-   Contenedor de equipos, que es la ubicación predeterminada para las nuevas cuentas de equipo creadas en el dominio

-   Uo controladores de dominio, que es la ubicación predeterminada para las cuentas de equipo para los controladores de dominio.

El propietario del bosque controla estos contenedores y unidades organizativas predeterminados.

## <a name="domain-container"></a>Contenedor de dominio
El contenedor de dominio es el contenedor raíz de la jerarquía de un dominio. Los cambios en las directivas o en la lista de control de acceso (ACL) de este contenedor pueden tener un impacto en todo el dominio. No delegue el control de este contenedor; debe ser controlado por los administradores del servicio.

## <a name="users-and-computers-containers"></a>Contenedores de usuarios y equipos
Cuando realiza una actualización de dominio local desde Windows Server 2003 a Windows Server 2008, los usuarios y equipos existentes se colocan automáticamente en los contenedores usuarios y equipos. Si va a crear un nuevo dominio de Active Directory, los contenedores usuarios y equipos son las ubicaciones predeterminadas para todas las cuentas de usuario nuevas y las cuentas de equipo que no son de controlador de dominio en el dominio.

> [!IMPORTANT]
> Si necesita delegar el control sobre usuarios o equipos, no modifique la configuración predeterminada en los contenedores usuarios y equipos. En su lugar, cree nuevas unidades organizativas (según sea necesario) y mueva los objetos de usuario y equipo de sus contenedores predeterminados y a las nuevas unidades organizativas. Delegue el control sobre las nuevas unidades organizativas, según sea necesario. Se recomienda no modificar quién controla los contenedores predeterminados.

Además, no se puede aplicar la configuración de directiva de grupo a los contenedores usuarios y equipos predeterminados. Para aplicar directiva de grupo a usuarios y equipos, cree nuevas unidades organizativas y mueva los objetos de usuario y equipo a esas unidades organizativas. Aplique la configuración de directiva de grupo a las nuevas unidades organizativas.

Opcionalmente, puede redirigir la creación de objetos que se colocan en los contenedores predeterminados para colocarlos en contenedores de su elección.

## <a name="well-known-users-and-groups-and-built-in-accounts"></a>Usuarios y grupos conocidos y cuentas integradas
De forma predeterminada, se crean varios usuarios y grupos conocidos y cuentas integradas en un nuevo dominio. Se recomienda que la administración de estas cuentas permanezca bajo el control de los administradores de servicios. No delegue la administración de estas cuentas a una persona que no sea un administrador de servicios. En la tabla siguiente se enumeran los usuarios y grupos conocidos y las cuentas integradas que deben permanecer bajo el control de los administradores de servicios.

|Usuarios y grupos conocidos|Cuentas integradas|
|--------------------------------|----------------------|
|Publicadores de certificados<p>Controladores de dominio<p>Propietarios del creador de directivas de grupo<p>KRBTGT<p>Invitados del dominio<p>Administrador<p>Administradores de dominio<p>Administradores de esquema (solo dominio raíz del bosque)<p>Administradores de organización (solo dominio raíz del bosque)<p>Usuarios del dominio|Administrador<p>Invitado<p>Invitados<p>Operadores de cuentas<p>Administradores<p>Operadores de copias de seguridad<p>Creadores de confianza de bosque de entrada<p>Opers. de impresión<p>Acceso compatible con versiones anteriores de Windows 2000<p>Operadores de servidores<p>Usuarios|

## <a name="domain-controller-ou"></a>Unidad organizativa del controlador de dominio
Cuando se agregan controladores de dominio al dominio, sus objetos de equipo se agregan automáticamente a la unidad organizativa del controlador de dominio. Esta unidad organizativa tiene un conjunto predeterminado de directivas aplicadas. Para asegurarse de que estas directivas se aplican uniformemente a todos los controladores de dominio, se recomienda no trasladar los objetos de equipo de los controladores de dominio de esta unidad organizativa. Si no se aplican las directivas predeterminadas, es posible que un controlador de dominio no funcione correctamente.

De forma predeterminada, los administradores de servicios controlan esta unidad organizativa. No delegue el control de esta unidad organizativa a individuos distintos de los administradores de servicios.



