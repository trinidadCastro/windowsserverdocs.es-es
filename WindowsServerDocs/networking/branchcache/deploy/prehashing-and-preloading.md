---
title: Aplicación de hash previo y carga previa de contenido en servidores de caché hospedada (opcional)
description: Obtenga información sobre cómo forzar la creación de información de contenido (también denominada hashes) en servidores web y de archivos habilitados para BranchCache.
manager: brianlic
ms.topic: how-to
ms.assetid: 5a09d9f1-1049-447f-a9bf-74adf779af27
ms.author: lizross
author: eross-msft
ms.date: 01/05/2021
ms.openlocfilehash: d903338f58cd7a01967902243f79f4eb28e58882
ms.sourcegitcommit: 40905b1f9d68f1b7d821e05cab2d35e9b425e38d
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/06/2021
ms.locfileid: "97942511"
---
# <a name="prehashing-and-preloading-content-on-hosted-cache-servers-optional"></a>Aplicación de hash previo y carga previa de contenido en servidores de caché hospedada (opcional)

>Se aplica a: Windows Server (canal semianual), Windows Server 2016

Puede usar este procedimiento para forzar la creación de información de contenido (también denominada hashes) en los servidores de archivos y web habilitados para BranchCache. También puede recopilar los datos de los servidores web y de archivos en los paquetes que se pueden transferir a los servidores de caché hospedada remota.  Esto le proporciona la capacidad de cargar previamente el contenido en servidores de caché hospedada remotamente para que los datos estén disponibles para el primer acceso de cliente.

Debe ser miembro de **los administradores** o equivalente para realizar este procedimiento.

### <a name="to-prehash-content-and-preload-the-content-on-hosted-cache-servers"></a>Para precifrar el contenido y cargar previamente el contenido en servidores de caché hospedada

1.  Inicie sesión en el servidor web o de archivos que contiene los datos que desea precargar e identifique las carpetas y los archivos que desea cargar en uno o varios servidores de caché hospedada remota.

2.  Ejecute Windows PowerShell como administrador. Para cada carpeta y archivo, ejecute el `Publish-BCFileContent` comando o el `Publish-BCWebContent` comando, en función del tipo de servidor de contenido, para desencadenar la generación de hash y agregar datos a un paquete de datos.

3.  Una vez que todos los datos se han agregado al paquete de datos, expórtelo mediante el `Export-BCCachePackage` comando para generar un archivo de paquete de datos.

4.  Mueva el archivo de paquete de datos a los servidores de caché hospedada remota mediante la tecnología de transferencia de archivos que prefiera.  FTP, SMB, HTTP, DVD y discos duros portátiles son transportes viables.

5.  Importe el archivo de paquete de datos en los servidores de caché hospedada remota mediante el `Import-BCCachePackage` comando.


