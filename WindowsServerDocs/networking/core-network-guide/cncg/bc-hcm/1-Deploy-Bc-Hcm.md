---
title: Implementar el modo de caché hospedada BranchCache
description: Esta guía brinda instrucciones sobre cómo implementar BranchCache en modo de memoria caché hospedada en equipos que ejecutan Windows Server 2016 y Windows 10
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking-bc
ms.topic: article
ms.assetid: 4235231c-4732-4ea9-9330-2a8c8a616d39
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 326a1f1edfe6cb763a33ebfc8fd5abdd5b6aab3a
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2018
---
# <a name="deploy-branchcache-hosted-cache-mode"></a>Implementar el modo de caché hospedada BranchCache

>Se aplica a: Windows Server (punto y anual canal), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

La Guía de red de Windows Server 2016 Core proporciona instrucciones para planear e implementar los componentes básicos necesarios para una red totalmente funcional y un nuevo Active Directory&reg; dominio en un bosque nuevo.

Esta guía explica cómo crear en la red principal, proporciona instrucciones para implementar BranchCache en modo de la memoria caché hospedada en uno o más sucursales con un controlador de dominio Read\-Only donde los equipos cliente ejecutan Windows&reg; 10, Windows 8.1 o Windows 8 y están unidos al dominio.

>[!IMPORTANT]
>Si vas a implementar o ya has implementado un servidor de caché BranchCache hospedado que ejecuta Windows Server 2008 R2, no uses a esta guía. Esta guía brinda instrucciones para implementar el modo de caché hospedada con un servidor de la memoria caché hospedada que ejecuta Windows Server&reg; de 2016, Windows Server 2012 R2 o Windows Server 2012.

Esta guía contiene las siguientes secciones.

- [Requisitos previos para usar esta guía](#bkmk_pre)

- [Acerca de esta guía](#bkmk_about)

- [Lo que no proporciona esta guía](#bkmk_not)

- [Información general de la tecnología](#bkmk_tech)

- [BranchCache hospedado caché Introducción al modo implementación](2-Bc-Hcm-Deploy-Overview.md)

- [BranchCache hospedado modo de caché de planeación de implementación](3-Bc-Hcm-Plan.md)

- [BranchCache hospedado implementación del modo de caché](4-Bc-Hcm-Deployment.md)

- [Recursos adicionales](11-Bc-Hcm-additional-resources.md)

## <a name="bkmk_pre"></a>Requisitos previos para usar esta guía

Esta es una guía de complemento a la Guía de red de Windows Server 2016 Core. Para implementar BranchCache en modo de la memoria caché hospedada con esta guía, primero debes hacer lo siguiente.

- Implementar una red principal en la oficina principal mediante la guía básica de red o ya han proporcionado las tecnologías en la guía básica de red instalado y funcione correctamente en la red. Estas tecnologías incluyen TCP\ TCP/IP v4, DHCP, los servicios de dominio de Active Directory \(AD DS\) y DNS.

    > [!NOTE]
    > El equipo con Windows Server 2016 [guía básica de red](https://technet.microsoft.com/windows-server-docs/networking/core-network-guide/core-network-guide) está disponible en la biblioteca técnica de Windows Server 2016.  

- Implementar BranchCache servidores que ejecutan Windows Server 2016, Windows Server 2012 R2 o Windows Server 2012 en la oficina principal o en un centro de datos de la nube. Para obtener información sobre cómo implementar los servidores de contenido BranchCache, vea [recursos adicionales](11-Bc-Hcm-additional-resources.md).

- Establecer \(WAN\) conexiones de red de área extensa entre tu sucursal, tu oficina principal y, si procede, los recursos de nube, mediante el uso de una privada virtual de red \(VPN\), DirectAccess u otro método de conexión.

- Implementar los equipos cliente en tu sucursal que se ejecutan uno de los siguientes sistemas operativos, que proporcionan BranchCache con compatibilidad para el servicio de transferencia inteligente en segundo plano (BITS), el protocolo de transferencia de hipertexto (HTTP) y el bloque de mensajes de servidor (SMB).
    - Windows 10 Enterprise
    - Windows 10 Education
    - Windows 8.1 Enterprise
    - Windows 8 Enterprise

>[!NOTE]
>En los siguientes sistemas operativos, BranchCache no admite la funcionalidad HTTP y SMB, pero es compatible con la funcionalidad de BranchCache BITS.
>     - Windows 10 Pro, BITS solo admiten
>     - Windows 8.1 Pro, BITS solo admiten
>     - Windows 8 Pro, BITS solo admiten

## <a name="bkmk_about"></a>Acerca de esta guía

Esta guía está diseñada para los administradores de red y del sistema que han seguido las instrucciones de la Guía de red principal de Windows Server 2016 o una guía de red principal de Windows Server 2012 para implementar una red principal, o para aquellos que han implementado previamente las tecnologías incluidas en la guía básica de red, incluidos los servicios de dominio de Active Directory \(AD DS\), \(DNS\) Domain Name Service, protocolo de configuración dinámica de Host \(DHCP\) y v4 TCP\ TCP/IP.

Se recomienda que revise a las guías de diseño e implementación para cada una de las tecnologías que se usan en este escenario de implementación. Estas guías pueden ayudarte a determinar si este escenario de implementación proporciona los servicios y la configuración que necesitas para la red de su organización.

## <a name="bkmk_not"></a>Lo que no proporciona esta guía

Esta guía no proporciona información conceptual sobre BranchCache, incluida la información sobre los modos de BranchCache y funcionalidades.  

Esta guía no proporciona información acerca de cómo implementar conexiones WAN u otras tecnologías en tu sucursal, como DHCP, un RODC o un servidor VPN.

Además, esta guía no proporciona orientación en el hardware que debes usar cuando implementas un servidor de la memoria caché hospedada. Es posible ejecutar otros servicios y aplicaciones en el servidor de la memoria caché hospedada, sin embargo, debes la determinación, en función de carga de trabajo, las funcionalidades de hardware y el tamaño de office de rama, si quieres instalar el servidor de caché BranchCache hospedado en un equipo determinado y cuánto espacio en disco para asignar a la memoria caché.  
Esta guía no proporciona instrucciones para configurar los equipos que ejecutan Windows 7. Si tienes equipos cliente que ejecutan Windows 7 en las sucursales, debes configurar mediante los procedimientos que sean diferentes a los que proporciona en esta guía para los equipos cliente que ejecutan Windows 10, Windows 8.1 y Windows 8.
  
Además, si tienes equipos que ejecutan Windows 7, debe configurar el servidor de la memoria caché hospedada con un certificado de servidor emitido por una entidad de certificación que confían en los equipos cliente. \ (Si todos los equipos cliente ejecutan Windows 10, Windows 8.1 o Windows 8, no es necesario configurar el servidor de la memoria caché hospedada con un certificado de servidor. \) 
> [!IMPORTANT]
> Si los servidores de la memoria caché hospedada ejecutan Windows Server 2008 R2, usa Windows Server 2008 R2 [Guía de implementación de BranchCache](https://technet.microsoft.com/library/ee649232(v=ws.10).aspx) en lugar de esta guía para implementar BranchCache en modo de la memoria caché hospedada. Aplicar la configuración de directiva de grupo que se describe en esa guía a todos los clientes BranchCache que ejecutan versiones de Windows desde Windows 7a Windows 10. No se puede configurar los equipos que ejecutan Windows Server 2008 R2 siguiendo los pasos de esta guía.

## <a name="bkmk_tech"></a>Información general de la tecnología

Para esta guía de inicio, BranchCache es la tecnología única que necesitas para instalar y configurar. Debes ejecutar los comandos de Windows PowerShell BranchCache en los servidores de archivos y los servidores de contenido, como la Web, pero no necesitas cambiar o volver a configurar los servidores de contenido de cualquier otra forma. Además, debes configurar los equipos cliente mediante la directiva de grupo en los controladores de dominio que ejecutan Windows Server 2012, Windows Server 2012 R2 o Windows Server 2016 AD DS.

### <a name="branchcache"></a>BranchCache

BranchCache es una tecnología de optimización de ancho de banda (WAN) de red de área extensa que se incluye en algunas ediciones de los sistemas operativos Windows Server 2016 y Windows 10, así como en algunas ediciones de Windows Server 2012 R2, Windows 8.1, Windows Server 2012, Windows 8, Windows Server 2008 R2 y Windows 7.

Para optimizar el ancho de banda WAN cuando los usuarios acceder a contenido en servidores remotos, BranchCache descarga contenido solicitadas por el cliente de la oficina principal u hospeda servidores de contenido de la nube y almacena en caché el contenido en sucursales, permitir que otros equipos de cliente en las sucursales para acceder al mismo contenido localmente en lugar de a través de la WAN.

Cuando implementas BranchCache en modo de caché hospedada, debes configurar los equipos cliente en las sucursales como los clientes de modo de memoria caché hospedada y, a continuación, debes implementar un servidor de la memoria caché hospedada en las sucursales. Esta guía muestra cómo implementar el servidor de la memoria caché hospedada con prehashed y precargada contenido desde la Web y servidores server\ contenidos del archivo.

### <a name="group-policy"></a>Directiva de grupo

Directiva de grupo en Windows Server 2012, Windows Server 2012 R2 y Windows Server 2016 es una infraestructura que se usa para entregar y aplicar configuraciones deseadas o configuración de directiva en un conjunto de dirigida a los usuarios y equipos dentro de un entorno de Active Directory. 

Esta infraestructura consta de un motor de directiva de grupo y varios \(CSEs\) extensiones del lado client\ que son responsables de leer la configuración de directiva en los equipos de cliente de destino.

Directiva de grupo se usa en este escenario para configurar equipos cliente miembros del dominio con el modo de caché BranchCache hospedado.

Para continuar con esta guía, consulte [BranchCache hospedado caché implementación introducción al modo](2-Bc-Hcm-Deploy-Overview.md).
