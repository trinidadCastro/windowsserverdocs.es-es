---
title: Instalar los servidores de contenido de los servicios de archivo
description: En este tema es parte de la BranchCache implementación Guía para Windows Server 2016, que se muestra cómo implementar BranchCache en modos de caché distribuida y hospedada para optimizar el uso de ancho de banda WAN en sucursales
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking-bc
ms.topic: get-started-article
ms.assetid: 74b0a5ed-dc20-4974-9d4b-2426987a01a1
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 7036323e7cc31bd14be8025b6064a806fb45ef19
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2018
---
# <a name="install-file-services-content-servers"></a>Instalar los servidores de contenido de los servicios de archivo

>Se aplica a: Windows Server (punto y anual canal), Windows Server 2016

Para implementar contenidos servidores que ejecutan el rol de servidor de servicios de archivo, debes instalar el BranchCache para el servicio de rol de archivos de red del rol de servidor de servicios de archivo. Además, debe habilitar BranchCache en recursos compartidos de archivos según los requisitos.  
  
Durante la configuración del servidor de contenido, puedes permitir que BranchCache publicación de contenido para todos los recursos compartidos de archivos o puedes seleccionar un subconjunto de recursos compartidos de archivos para publicar.  
  
> [!NOTE]  
> Cuando implementas un servidor de archivos habilitados BranchCache o Web como un servidor de contenido, información sobre el contenido ahora se calcula sin conexión, mucho antes de que un cliente BranchCache solicita un archivo. Debido a esta mejora, no es necesario configurar hash de publicación para servidores de contenido, como se hizo en la versión anterior de BranchCache.  
>   
> Esta generación automática de hash proporciona un rendimiento más rápido y más ahorro de ancho de banda, porque la información sobre el contenido está listo para el primer cliente que solicita el contenido, y ya se han realizado cálculos.  
  
Consulta los temas siguientes para implementar los servidores de contenido.  
  
-   [Configurar el rol de servidor de servicios de archivos](../../branchcache/deploy/Configure-the-File-Services-server-role.md)  
  
-   [Habilitar el Hash de publicación para servidores de archivos](../../branchcache/deploy/Enable-Hash-Publication-for-File-Servers.md)  
  
-   [Habilitar BranchCache en un recurso compartido de archivos & #40; opcional & #41;](../../branchcache/deploy/enable-bc-on-file-share.md)  
  


