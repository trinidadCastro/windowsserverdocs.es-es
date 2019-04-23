---
title: Aplicación de hash previo y carga previa de contenido en servidores de caché hospedada (opcional)
description: En este tema forma parte de BranchCache Deployment Guide para Windows Server 2016, que demuestra cómo implementar BranchCache en los modos de caché distribuida y hospedada para optimizar el uso de ancho de banda WAN de sucursales.
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking-bc
ms.topic: get-started-article
ms.assetid: 5a09d9f1-1049-447f-a9bf-74adf779af27
ms.author: pashort
author: shortpatti
ms.openlocfilehash: b421132a44240520e3e3ba294623584c36b18ab4
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59867006"
---
# <a name="prehashing-and-preloading-content-on-hosted-cache-servers-optional"></a>Aplicación de hash previo y carga previa de contenido en servidores de caché hospedada (opcional)

>Se aplica a: Windows Server (canal semianual), Windows Server 2016

Puede usar este procedimiento para forzar la creación de información de contenido - llamada también hashes - en los servidores Web y de archivos habilitado para BranchCache. También puede recopilar los datos en servidores de archivos y web en paquetes que se pueden transferir a servidores de caché hospedada remoto.  Esto le brinda la capacidad para cargar previamente contenido en servidores de caché hospedada remoto para que los datos están disponibles para el primer acceso de cliente.  
  
Debe ser miembro del **administradores**, o equivalente para realizar este procedimiento.  
  
### <a name="to-prehash-content-and-preload-the-content-on-hosted-cache-servers"></a>Para hacer un hash previo contenido y precargar el contenido en servidores de caché hospedada  
  
1.  Inicie sesión en el archivo o el servidor Web que contiene los datos que desea cargar previamente e identifique las carpetas y archivos que desea cargar en uno o más servidores de caché hospedada remoto.  
  
2.  Ejecutar Windows PowerShell como administrador. Para cada carpeta y archivo, ejecute el `Publish-BCFileContent` comando o la `Publish-BCWebContent` comando, según el tipo de servidor de contenido, para desencadenar la generación de hash y para agregar datos a un paquete de datos.  
  
3.  Después de agregar todos los datos al paquete de datos, exportar mediante la `Export-BCCachePackage` comando para generar un archivo de paquete de datos.  
  
4.  Mueva el archivo de paquete de datos a los servidores de caché hospedada remoto mediante el uso de su elección de tecnología de transferencia de archivos.  FTP, HTTP, SMB, DVD y discos duros portátiles son viables todos los transportes.  
  
5.  Importar el archivo de paquete de datos en los servidores de caché hospedada remoto mediante el `Import-BCCachePackage` comando.  
  

