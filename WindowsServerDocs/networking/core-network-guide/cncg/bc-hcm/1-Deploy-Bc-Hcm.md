---
title: Implementar el modo de caché hospedada de BranchCache
description: Esta guía proporciona instrucciones sobre cómo implementar BranchCache en modo de caché hospedada en equipos que ejecutan Windows Server 2016 y Windows 10
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking-bc
ms.topic: article
ms.assetid: 4235231c-4732-4ea9-9330-2a8c8a616d39
ms.author: pashort
author: shortpatti
ms.openlocfilehash: dc2cb29f0f00c04c4208bd83d70bc4d966bbad00
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59839356"
---
# <a name="deploy-branchcache-hosted-cache-mode"></a>Implementar el modo de caché hospedada de BranchCache

>Se aplica a: Windows Server (canal semianual), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

La Guía de red de Windows Server 2016 Core proporciona instrucciones para planear e implementar los componentes principales necesarios para una red plenamente funcional y un nuevo Active Directory&reg; dominio en un bosque nuevo.

Esta guía explica cómo basarse en la red principal proporcionando instrucciones para implementar BranchCache en modo de caché hospedada en uno o más sucursales con una lectura\-solo controlador de dominio donde los equipos cliente ejecutan Windows&reg; 10, Windows 8.1 o Windows 8 y están unidos al dominio.

>[!IMPORTANT]
>No use esta guía si va a implementar o ya ha implementado un servidor de caché hospedada de BranchCache que se está ejecutando Windows Server 2008 R2. Esta guía proporciona instrucciones para implementar el modo de caché hospedada con un servidor de caché hospedada que se está ejecutando Windows Server&reg; 2016, Windows Server 2012 R2 o Windows Server 2012.

En esta guía se incluyen las siguientes secciones.

- [Requisitos previos para usar esta guía](#bkmk_pre)

- [Acerca de esta guía](#bkmk_about)

- [Qué no incluye esta guía](#bkmk_not)

- [Introducción a las tecnologías](#bkmk_tech)

- [Información general de implementación de modo caché hospedada de BranchCache](2-Bc-Hcm-Deploy-Overview.md)

- [El modo caché hospedada de BranchCache planeación de la implementación](3-Bc-Hcm-Plan.md)

- [Implementación en modo caché hospedada de BranchCache](4-Bc-Hcm-Deployment.md)

- [Recursos adicionales](11-Bc-Hcm-additional-resources.md)

## <a name="bkmk_pre"></a>Requisitos previos para usar esta guía

Se trata de una guía complementaria a la Guía de red de Windows Server 2016 Core. Para implementar BranchCache en modo de caché hospedada con esta guía, primero tienes que hacer lo siguiente.

- Implementa una red principal en tu oficina central usando la Guía de red principal, o ten instaladas y funcionando correctamente en tu red las tecnologías proporcionadas en la Guía de red principal de antemano. Estas tecnologías incluyen TCP\/IP v4, DHCP, Active Directory Domain Services \(AD DS\)y DNS.

    > [!NOTE]
    > Windows Server 2016 [Guía de red principal](https://technet.microsoft.com/windows-server-docs/networking/core-network-guide/core-network-guide) está disponible en la biblioteca técnica de Windows Server 2016.  

- Implementar servidores de contenido de BranchCache que ejecutan Windows Server 2016, Windows Server 2012 R2 o Windows Server 2012 en la oficina central o en un centro de datos en la nube. Para obtener información sobre cómo implementar servidores de contenido de BranchCache, vea [recursos adicionales](11-Bc-Hcm-additional-resources.md).

- Establecer la red de área extensa \(WAN\) conexiones entre tu sucursal, tu oficina central y, si procede, los recursos de nube, mediante el uso de una red privada virtual \(VPN\), DirectAccess, u otros método de conexión.

- Implementar equipos cliente en la sucursal que ejecutan uno de los siguientes sistemas operativos, que proporcionan BranchCache con compatibilidad con el servicio de transferencia inteligente en segundo plano (BITS), protocolo de transferencia de hipertexto (HTTP) y bloque de mensajes del servidor (SMB) .
    - Windows 10 Enterprise
    - Windows 10 Education
    - Windows 8.1 Enterprise
    - Windows 8 Enterprise

>[!NOTE]
>En los siguientes sistemas operativos, BranchCache no admite la funcionalidad HTTP y SMB, pero es compatible con la funcionalidad de BranchCache BITS.
>     - Windows 10 Pro, BITS solamente es compatible con
>     - Windows 8.1 Pro, BITS solamente es compatible con
>     - Windows 8 Pro, BITS solamente es compatible con

## <a name="bkmk_about"></a>Acerca de esta guía

Esta guía está diseñada para los administradores de red y del sistema que han seguido las instrucciones de la Guía de red de Windows Server 2016 Core o la Guía de red de Windows Server 2012 Core para implementar una red principal, o para aquellos que previamente ha implementado el tecnologías incluidas en la Guía de red principal, incluidos los servicios de dominio de Active Directory \(AD DS\), servicio de nombres de dominio \(DNS\), Dynamic Host Configuration Protocol \(DHCP\)y TCP\/IP v4.

Se recomienda que revises las guías de diseño e implementación de todas las tecnologías que se usan en este escenario de implementación. Estas guías pueden ayudarte a determinar si este escenario de implementación ofrece los servicios y la configuración que necesitas para la red de tu organización.

## <a name="bkmk_not"></a>¿Qué incluye esta guía no

Esta guía no proporciona información conceptual sobre BranchCache, incluyendo la información sobre los modos y capacidades de BranchCache.  

Esta guía no proporciona información sobre cómo implementar conexiones WAN u otras tecnologías de tu sucursal, como DHCP, un RODC o un servidor VPN.

Además, esta guía no proporciona orientación sobre el hardware que debes usar cuando implementas un servidor de caché hospedada. Es posible ejecutar otros servicios y aplicaciones en tu servidor de caché hospedada, sin embargo tienes que tomar la determinación, según la carga de trabajo, las capacidades de hardware y el tamaño de la sucursal, de si quieres instalar el servidor de caché hospedada de BranchCache en un equipo determinado y cuánto espacio en disco asignar a la caché.  
Esta guía no proporciona instrucciones para configurar los equipos que ejecutan Windows 7. Si tiene equipos cliente que ejecutan Windows 7 en sus sucursales, debe configurarlos mediante procedimientos distintos de los proporcionados en esta guía para los equipos cliente que ejecutan Windows 10, Windows 8.1 y Windows 8.
  
Además, si tiene equipos que ejecutan Windows 7, debe configurar el servidor de caché hospedada con un certificado de servidor emitido por una entidad de certificación que confían en los equipos cliente. \(Si todos los equipos cliente ejecutan Windows 10, Windows 8.1 o Windows 8, no es necesario configurar el servidor de caché hospedada con un certificado de servidor.\) 
> [!IMPORTANT]
> Si los servidores de caché hospedada se ejecuta Windows Server 2008 R2, use Windows Server 2008 R2 [BranchCache Deployment Guide](https://technet.microsoft.com/library/ee649232(v=ws.10).aspx) en lugar de esta guía para implementar BranchCache en modo caché hospedada. Aplicar la configuración de directiva de grupo que se describe en esa guía a todos los clientes de BranchCache que se ejecutan las versiones de Windows desde Windows 7 a Windows 10. No se puede configurar equipos que ejecutan Windows Server 2008 R2 mediante los pasos de esta guía.

## <a name="bkmk_tech"></a>Introducción a las tecnologías

En esta guía complementaria, BranchCache es la única tecnología que necesitas para la instalación y configuración. Tienes que ejecutar los comandos de Windows PowerShell BranchCache en los servidores de contenido, como los servidores web y de archivos, sin embargo, no necesitas cambiar o volver a configurar los servidores de contenido de ninguna otra forma. Además, debe configurar los equipos cliente mediante la directiva de grupo en los controladores de dominio que ejecutan AD DS en Windows Server 2016, Windows Server 2012 R2 o Windows Server 2012.

### <a name="branchcache"></a>BranchCache

BranchCache es una tecnología de optimización de ancho de banda (WAN) de red de área extensa que se incluye en algunas ediciones de los sistemas de operativos de Windows 10 y Windows Server 2016, así como en algunas ediciones de Windows Server 2012 R2, Windows 8.1, Windows Server 2012, Windows 8 , Windows Server 2008 R2 y Windows 7.

Para optimizar el ancho de banda WAN cuando los usuarios tienen acceso a contenido en servidores remotos, BranchCache descarga contenido solicitada por el cliente de la oficina central o servidores de contenido en la nube hospedada y almacena en caché el contenido en ubicaciones de sucursales, lo que permite a otros equipos cliente en las sucursales para acceder al contenido mismo localmente en lugar de a través de la WAN.

Cuando implementas BranchCache en modo de caché hospedada, tienes que configurar los equipos cliente de la sucursal como clientes del modo de caché hospedada y después tienes que implementar un servidor de caché hospedada en la sucursal. Esta guía muestra cómo implementar el servidor de caché hospedada con contenido hash y cargado previamente desde el servidor Web y el archivo\-en función de los servidores de contenido.

### <a name="group-policy"></a>Directiva de grupo

Directiva de grupo en Windows Server 2016, Windows Server 2012 R2 y Windows Server 2012 es una infraestructura que se usa para proporcionar y aplicar una o varias deseado o configuraciones de directiva a un conjunto de usuarios de destino y los equipos dentro de un entorno de Active Directory. 

Esta infraestructura consta de un motor de directiva de grupo y varias cliente\-las extensiones del lado \(CSE\) que son responsables de leer configuraciones de directiva en los equipos cliente de destino.

En este escenario se usa la directiva de grupo para configurar equipos cliente miembros del dominio con el modo de caché hospedada de BranchCache.

Para continuar con esta guía, consulte [hospedado caché de modo de implementación información general sobre BranchCache](2-Bc-Hcm-Deploy-Overview.md).
