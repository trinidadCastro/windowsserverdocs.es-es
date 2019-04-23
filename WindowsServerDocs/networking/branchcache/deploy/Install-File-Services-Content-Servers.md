---
title: Instalar servidores de contenido de Servicios de archivo
description: En este tema forma parte de BranchCache Deployment Guide para Windows Server 2016, que demuestra cómo implementar BranchCache en los modos de caché distribuida y hospedada para optimizar el uso de ancho de banda WAN de sucursales
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking-bc
ms.topic: get-started-article
ms.assetid: 74b0a5ed-dc20-4974-9d4b-2426987a01a1
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 496c0c1408c64216f29a31d5b22d3d9b48d4f44c
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59855066"
---
# <a name="install-file-services-content-servers"></a>Instalar servidores de contenido de Servicios de archivo

>Se aplica a: Windows Server (canal semianual), Windows Server 2016

Para implementar servidores de contenido que ejecutan el rol de servidor Servicios de archivo, debe instalar el servicio de rol BranchCache para archivos de red del rol de servidor Servicios de archivo. Además, debe habilitar BranchCache en recursos compartidos de archivos según sus requisitos.  
  
Durante la configuración del servidor de contenido, puede permitir la publicación de contenido de BranchCache para todos los recursos compartidos de archivos, o puede seleccionar un subconjunto de los recursos compartidos de archivos para su publicación.  
  
> [!NOTE]  
> Al implementar un servidor de archivos habilitado BranchCache o el servidor Web como un servidor de contenido, la información de contenido ahora se calcula sin conexión, mucho antes de que un cliente de BranchCache solicite un archivo. Debido a esta mejora, no es necesario configurar la publicación de hash para servidores de contenido, como hizo en la versión anterior de BranchCache.  
>   
> Esta generación automática de hash proporciona un rendimiento más rápido y más ahorro de ancho de banda, porque la información de contenido está lista para el primer cliente que solicita el contenido y ya se han realizado los cálculos.  
  
Vea los temas siguientes para implementar servidores de contenido.  
  
-   [Configurar el rol de servidor Servicios de archivo](../../branchcache/deploy/Configure-the-File-Services-server-role.md)  
  
-   [Habilitar la publicación de Hash para servidores de archivos](../../branchcache/deploy/Enable-Hash-Publication-for-File-Servers.md)  
  
-   [Habilitar BranchCache en un recurso compartido de archivos &#40;opcional&#41;](../../branchcache/deploy/enable-bc-on-file-share.md)  
  


