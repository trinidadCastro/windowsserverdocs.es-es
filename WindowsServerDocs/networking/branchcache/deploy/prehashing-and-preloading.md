---
title: Prehashing y precargar contenido en los servidores de la memoria caché hospedada (opcionales)
description: Este tema es parte de la BranchCache implementación Guía para Windows Server 2016, que se muestra cómo implementar BranchCache en modos de caché distribuida y hospedada para optimizar el uso de ancho de banda WAN en sucursales.
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking-bc
ms.topic: get-started-article
ms.assetid: 5a09d9f1-1049-447f-a9bf-74adf779af27
ms.author: pashort
author: shortpatti
ms.openlocfilehash: c3d1ed62c6dca5b1de0ff560fde0a2e43ed0d080
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2018
---
# <a name="prehashing-and-preloading-content-on-hosted-cache-servers-optional"></a>Prehashing y precargar contenido en los servidores de la memoria caché hospedada (opcionales)

>Se aplica a: Windows Server (punto y anual canal), Windows Server 2016

Puedes usar este procedimiento para forzar la creación de información de contenido, también denominado hashes - en servidores Web y de archivo compatibles con BranchCache. También puedes recopilar los datos en los servidores web y archivos en paquetes que se pueden transferir a los servidores remotos de memoria caché hospedada.  Esto proporciona la capacidad de precargar contenido en los servidores remotos de memoria caché hospedada para que los datos están disponibles para el primer acceso de cliente.  
  
Debe ser miembro del **administradores**, o equivalente para realizar este procedimiento.  
  
### <a name="to-prehash-content-and-preload-the-content-on-hosted-cache-servers"></a>Prehash contenido y cargar el contenido en los servidores de la memoria caché hospedada  
  
1.  Iniciar sesión en el archivo o servidor Web que contiene los datos que desea precargar e identificar las carpetas y archivos que quieres cargar en uno o varios servidores remotos de memoria caché hospedada.  
  
2.  Ejecutar Windows PowerShell como administrador. Para cada carpeta y archivo, ejecuta el `Publish-BCFileContent`comando o la `Publish-BCWebContent`comando, según el tipo de servidor de contenido, para desencadenar la generación de hash y agregar datos a un paquete de datos.  
  
3.  Después de que todos los datos se haya agregado al paquete de datos, exportar mediante la `Export-BCCachePackage`comando para generar un archivo de paquete de datos.  
  
4.  Mover el archivo de paquete de datos a los servidores remotos de memoria caché hospedada mediante la elección de tecnología de transferencia de archivos.  FTP, SMB, HTTP, DVD y discos duros son viables todos los transportes.  
  
5.  Importar el archivo de paquete de datos en los servidores remotos de memoria caché hospedada mediante la `Import-BCCachePackage`comando.  
  

