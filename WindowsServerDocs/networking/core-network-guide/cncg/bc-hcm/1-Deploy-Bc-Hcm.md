---
title: Implementar el modo de caché hospedada de BranchCache
description: En esta guía se proporcionan instrucciones sobre cómo implementar BranchCache en modo caché hospedada en equipos que ejecutan Windows Server 2016 y Windows 10.
manager: brianlic
ms.prod: windows-server
ms.technology: networking-bc
ms.topic: article
ms.assetid: 4235231c-4732-4ea9-9330-2a8c8a616d39
ms.author: lizross
author: eross-msft
ms.openlocfilehash: 1da6df19933d3a4b9866b0428fb0088ac5f862b9
ms.sourcegitcommit: da7b9bce1eba369bcd156639276f6899714e279f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/26/2020
ms.locfileid: "80319093"
---
# <a name="deploy-branchcache-hosted-cache-mode"></a>Implementar el modo de caché hospedada de BranchCache

>Se aplica a: Windows Server (canal semianual), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

La guía de red principal de Windows Server 2016 proporciona instrucciones para planear e implementar los componentes principales necesarios para una red totalmente operativa y un nuevo Active Directory&reg; dominio en un bosque nuevo.

En esta guía se explica cómo basarse en la red principal proporcionando instrucciones para implementar BranchCache en modo caché hospedada en una o varias sucursales con un solo controlador de dominio de lectura\-en el que los equipos cliente ejecutan Windows&reg; 10, Windows 8.1 o Windows 8 y se unen al dominio.

>[!IMPORTANT]
>No use esta guía si planea implementar o ya ha implementado un servidor de caché hospedada de BranchCache que ejecuta Windows Server 2008 R2. En esta guía se proporcionan instrucciones para implementar el modo caché hospedada con un servidor de caché hospedada que ejecuta Windows Server&reg; 2016, Windows Server 2012 R2 o Windows Server 2012.

En esta guía se incluyen las siguientes secciones.

- [Requisitos previos para usar esta guía](#bkmk_pre)

- [Acerca de esta guía](#bkmk_about)

- [Qué no incluye esta guía](#bkmk_not)

- [Información general sobre tecnología](#bkmk_tech)

- [Información general de implementación de modo de caché hospedada de BranchCache](2-Bc-Hcm-Deploy-Overview.md)

- [Planeación de implementación de modo caché hospedada de BranchCache](3-Bc-Hcm-Plan.md)

- [Implementación del modo de caché hospedada de BranchCache](4-Bc-Hcm-Deployment.md)

- [Recursos adicionales](11-Bc-Hcm-additional-resources.md)

## <a name="prerequisites-for-using-this-guide"></a><a name="bkmk_pre"></a>Requisitos previos para usar esta guía

Esta es una guía complementaria de la guía de red principal de Windows Server 2016. Para implementar BranchCache en modo de caché hospedada con esta guía, primero tienes que hacer lo siguiente.

- Implementa una red principal en tu oficina central usando la Guía de red principal, o ten instaladas y funcionando correctamente en tu red las tecnologías proporcionadas en la Guía de red principal de antemano. Estas tecnologías incluyen TCP\/IP V4, DHCP, Active Directory Domain Services \(AD DS\)y DNS.

    > [!NOTE]
    > La [Guía de red principal](https://technet.microsoft.com/windows-server-docs/networking/core-network-guide/core-network-guide) de windows Server 2016 está disponible en la biblioteca técnica de windows Server 2016.  

- Implemente servidores de contenido de BranchCache que ejecuten Windows Server 2016, Windows Server 2012 R2 o Windows Server 2012 en la oficina central o en un centro de datos en la nube. Para obtener información sobre cómo implementar servidores de contenido de BranchCache, consulte [recursos adicionales](11-Bc-Hcm-additional-resources.md).

- Establezca redes de área extensa \(WAN\) conexiones entre la sucursal, su oficina principal y, si es necesario, los recursos en la nube mediante una red privada virtual \(VPN\), DirectAccess u otro método de conexión.

- Implemente los equipos cliente de la sucursal que ejecuten uno de los siguientes sistemas operativos, que proporcionan a BranchCache compatibilidad con Servicio de transferencia inteligente en segundo plano (BITS), el protocolo de transferencia de hipertexto (HTTP) y el bloque de mensajes del servidor (SMB). .
    - Windows 10 Enterprise
    - Windows 10 Education
    - Windows 8,1 Enterprise
    - Windows 8 Enterprise

> [!NOTE]
> En los siguientes sistemas operativos, BranchCache no admite la funcionalidad de HTTP y SMB, pero admite la funcionalidad de BITS de BranchCache.
>     - Windows 10 Pro, solo compatible con BITS
>     - Windows 8.1 Pro, solo compatible con BITS
>     - Windows 8 Pro, solo compatible con BITS

## <a name="about-this-guide"></a><a name="bkmk_about"></a>Acerca de esta guía

Esta guía está diseñada para administradores de red y de sistema que han seguido las instrucciones de la guía de red principal de Windows Server 2016 o la guía de red principal de Windows Server 2012 para implementar una red principal. o para aquellos que han implementado previamente las tecnologías incluidas en la guía de red principal, como Active Directory Domain Services \(AD DS\), el servicio de nombres de dominio \(DNS\), el protocolo de configuración dinámica de host \(DHCP\)y TCP\/IP V4.

Se recomienda que revises las guías de diseño e implementación de todas las tecnologías que se usan en este escenario de implementación. Estas guías pueden ayudarte a determinar si este escenario de implementación ofrece los servicios y la configuración que necesitas para la red de tu organización.

## <a name="what-this-guide-does-not-provide"></a><a name="bkmk_not"></a>Qué no proporciona esta guía

Esta guía no proporciona información conceptual sobre BranchCache, incluyendo la información sobre los modos y capacidades de BranchCache.  

Esta guía no proporciona información sobre cómo implementar conexiones WAN u otras tecnologías de tu sucursal, como DHCP, un RODC o un servidor VPN.

Además, esta guía no proporciona orientación sobre el hardware que debes usar cuando implementas un servidor de caché hospedada. Es posible ejecutar otros servicios y aplicaciones en tu servidor de caché hospedada, sin embargo tienes que tomar la determinación, según la carga de trabajo, las capacidades de hardware y el tamaño de la sucursal, de si quieres instalar el servidor de caché hospedada de BranchCache en un equipo determinado y cuánto espacio en disco asignar a la caché.  
En esta guía no se proporcionan instrucciones para configurar equipos que ejecutan Windows 7. Si tiene equipos cliente que ejecutan Windows 7 en sus sucursales, debe configurarlos mediante procedimientos distintos de los proporcionados en esta guía para los equipos cliente que ejecutan Windows 10, Windows 8.1 y Windows 8.
  
Además, si tiene equipos que ejecutan Windows 7, debe configurar el servidor de caché hospedada con un certificado de servidor emitido por una entidad de certificación en la que confíen los equipos cliente. \(si todos los equipos cliente ejecutan Windows 10, Windows 8.1 o Windows 8, no es necesario configurar el servidor de caché hospedada con un certificado de servidor.\) 
> [!IMPORTANT]
> Si los servidores de caché hospedada ejecutan Windows Server 2008 R2, use la guía de [implementación de BranchCache](https://technet.microsoft.com/library/ee649232(v=ws.10).aspx) de windows Server 2008 R2 en lugar de esta guía para implementar BranchCache en modo caché hospedada. Aplique la configuración de directiva de grupo que se describe en esa guía a todos los clientes de BranchCache que ejecutan versiones de Windows de Windows 7 a Windows 10. Los equipos que ejecutan Windows Server 2008 R2 no se pueden configurar siguiendo los pasos de esta guía.

## <a name="technology-overviews"></a><a name="bkmk_tech"></a>Información general sobre tecnología

En esta guía complementaria, BranchCache es la única tecnología que necesitas para la instalación y configuración. Tienes que ejecutar los comandos de Windows PowerShell BranchCache en los servidores de contenido, como los servidores web y de archivos, sin embargo, no necesitas cambiar o volver a configurar los servidores de contenido de ninguna otra forma. Además, debe configurar los equipos cliente mediante directiva de grupo en los controladores de dominio que ejecutan AD DS en Windows Server 2016, Windows Server 2012 R2 o Windows Server 2012.

### <a name="branchcache"></a>BranchCache

BranchCache es una tecnología de optimización del ancho de banda de la red de área extensa (WAN) que se incluye en algunas ediciones de los sistemas operativos Windows Server 2016 y Windows 10, así como en algunas ediciones de Windows Server 2012 R2, Windows 8.1, Windows Server 2012, Windows 8 , Windows Server 2008 R2 y Windows 7.

Para optimizar el ancho de banda WAN cuando los usuarios acceden al contenido de servidores remotos, BranchCache descarga el contenido solicitado por el cliente desde la oficina principal o los servidores de contenido en la nube hospedados, y almacena en caché el contenido en las sucursales, lo que permite que otros equipos cliente en las sucursales tienen acceso al mismo contenido de forma local en lugar de a través de la WAN.

Cuando implementas BranchCache en modo de caché hospedada, tienes que configurar los equipos cliente de la sucursal como clientes del modo de caché hospedada y después tienes que implementar un servidor de caché hospedada en la sucursal. En esta guía se muestra cómo implementar el servidor de caché hospedada con contenido precifrado y precargado de su servidor Web y\-servidores de contenido basados en.

### <a name="group-policy"></a>Directiva de grupo

Directiva de grupo en Windows Server 2016, Windows Server 2012 R2 y Windows Server 2012 es una infraestructura que se usa para proporcionar y aplicar una o varias configuraciones o configuraciones de directiva deseadas a un conjunto de usuarios y equipos de destino dentro de un entorno de Active Directory. 

Esta infraestructura se compone de un motor de directiva de grupo y varias extensiones del lado cliente\-\(CSE\) que son responsables de leer la configuración de la Directiva en los equipos cliente de destino.

En este escenario se usa la directiva de grupo para configurar equipos cliente miembros del dominio con el modo de caché hospedada de BranchCache.

Para continuar con esta guía, consulte [información general sobre la implementación del modo caché hospedada de BranchCache](2-Bc-Hcm-Deploy-Overview.md).
