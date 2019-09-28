---
title: Aplicación de hash previo y carga previa de contenido en servidores de caché hospedada (opcional)
description: Este tema forma parte de la guía de implementación de BranchCache para Windows Server 2016, que muestra cómo implementar BranchCache en los modos de caché distribuida y hospedada para optimizar el uso del ancho de banda WAN en las sucursales.
manager: brianlic
ms.prod: windows-server
ms.technology: networking-bc
ms.topic: get-started-article
ms.assetid: 5a09d9f1-1049-447f-a9bf-74adf779af27
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 1a0694ebe30c5a927fb2f1152adc792f8b0c9157
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71356393"
---
# <a name="prehashing-and-preloading-content-on-hosted-cache-servers-optional"></a>Aplicación de hash previo y carga previa de contenido en servidores de caché hospedada (opcional)

>Se aplica a: Windows Server (canal semianual), Windows Server 2016

Puede usar este procedimiento para forzar la creación de información de contenido (también denominada hashes) en los servidores de archivos y web habilitados para BranchCache. También puede recopilar los datos de los servidores web y de archivos en los paquetes que se pueden transferir a los servidores de caché hospedada remota.  Esto le proporciona la capacidad de cargar previamente el contenido en servidores de caché hospedada remotamente para que los datos estén disponibles para el primer acceso de cliente.  
  
Debe ser miembro de **los administradores**o equivalente para realizar este procedimiento.  
  
### <a name="to-prehash-content-and-preload-the-content-on-hosted-cache-servers"></a>Para precifrar el contenido y cargar previamente el contenido en servidores de caché hospedada  
  
1.  Inicie sesión en el servidor web o de archivos que contiene los datos que desea precargar e identifique las carpetas y los archivos que desea cargar en uno o varios servidores de caché hospedada remota.  
  
2.  Ejecute Windows PowerShell como administrador. Para cada carpeta y archivo, ejecute el comando `Publish-BCFileContent` o el comando `Publish-BCWebContent`, en función del tipo de servidor de contenido, para desencadenar la generación de hash y agregar datos a un paquete de datos.  
  
3.  Una vez que todos los datos se han agregado al paquete de datos, expórtelo mediante el comando `Export-BCCachePackage` para generar un archivo de paquete de datos.  
  
4.  Mueva el archivo de paquete de datos a los servidores de caché hospedada remota mediante la tecnología de transferencia de archivos que prefiera.  FTP, SMB, HTTP, DVD y discos duros portátiles son transportes viables.  
  
5.  Importe el archivo de paquete de datos en los servidores de caché hospedada remota mediante el comando `Import-BCCachePackage`.  
  

