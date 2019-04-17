---
title: "Introducción al Administrador de recursos del servidor de archivos (FSRM)"
ms.prod: windows-server-threshold
ms.author: jgerend
ms.manager: brianlic
ms.technology: storage
ms.topic: article
author: jasongerend
ms.date: 7/7/2017
description: El Administrador de recursos del servidor de archivos (FSRM) es una herramienta que te permite administrar y clasificar los datos de un servidor de archivos de WindowsServer.
ms.openlocfilehash: ddfc0a0f4bede89a3c3a624d4f128717d0f1ac08
ms.sourcegitcommit: 583355400f6b0d880dc0ac6bc06f0efb50d674f7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/17/2017
---
# <a name="file-server-resource-manager-fsrm-overview"></a>Introducción al Administrador de recursos del servidor de archivos (FSRM)

> Se aplica a: Windows Server (canal semianual), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2

El Administrador de recursos del servidor (FSRM) es un servicio de rol de WindowsServer que te permite administrar y clasificar los datos almacenados en los servidores de archivos. El Administrador de recursos del servidor de archivos incluye las siguientes características:
  
-   **Infraestructura de clasificación de archivos** : la infraestructura de clasificación de archivos permite obtener una idea clara de los datos mediante la automatización de los procesos de clasificación con el fin de poder administrar los datos de manera más eficaz. Puede clasificar archivos y aplicar directivas según esta clasificación. Entre las directivas de ejemplo está el control de acceso dinámico para restringir el acceso a archivos, el cifrado de archivos y la expiración de archivos. Los archivos pueden clasificarse de manera automática mediante reglas de clasificación de archivos o de forma manual, modificando las propiedades de un archivo o carpeta que se haya seleccionado.  
  
-   **Tareas de administración de archivos** : las tareas de administración de archivos permiten aplicar una acción o directiva condicional en los archivos según su clasificación. Entre las condiciones de una tarea de administración de archivos están la ubicación del archivo, las propiedades de clasificación, la fecha en que se creó el archivo, la fecha de la última modificación del archivo o la última vez que se accedió al archivo. Entre las acciones que una tarea de administración de archivos puede llevar a cabo se incluyen la capacidad de expirar archivos, cifrar archivos o ejecutar un comando personalizado.  
  
-   **Administración de cuotas** : las cuotas limitan el espacio permitido para un volumen o carpeta y se pueden aplicar automáticamente a las nuevas carpetas que se creen en un volumen. También puedes definir plantillas de cuota que se apliquen a los nuevos volúmenes o carpetas.  
  
-   **Administración del filtrado de archivos** : el filtrado de archivos ayuda a controlar los tipos de archivos que el usuario puede almacenar en un servidor de archivos. Puede limitar la extensión que se puede almacenar en los archivos compartidos. Por ejemplo, puede crear un filtrado de archivos que no permita que se almacenen archivos con extensiónMP3 en las carpetas compartidas personales en un servidor de archivos.  
  
-   **Informes de almacenamiento** : los informes de almacenamiento sirven para identificar las tendencias de uso del disco y el modo en que los datos están clasificados. También puedes supervisar un grupo de usuarios en concreto para detectar los intentos de guardar archivos no autorizados.  
  
 Las características incluidas en el Administrador de recursos del servidor de archivos se pueden configurar y administrar mediante Microsoft Management Console (MMC) del Administrador de recursos del servidor de archivos o usando WindowsPowerShell.  
  
> [!IMPORTANT]
>  El Administrador de recursos del servidor de archivos admite únicamente volúmenes que estén formateados con el sistema de archivos NTFS. El sistema de archivos resistente no se admite.  
  
## <a name="practical-applications"></a>Aplicaciones prácticas  
 Entre las aplicaciones prácticas del Administrador de recursos del servidor de archivos se incluyen las siguientes:  
  
-   Usa la Infraestructura de clasificación de archivos con el escenario de Control de acceso dinámico para crear una directiva que conceda acceso a los archivos y carpetas en función del modo en que se clasifican los archivos en el servidor de archivos.  
  
-   Crea una regla de clasificación de archivos que etiquete cualquier archivo que contenga al menos 10 números de la seguridad social como archivo que contiene información de identificación personal.  
  
-   Establece la expiración de cualquier archivo que no se haya modificado en los últimos 10años.  
  
-   Establece una cuota de 200megabytes para el directorio principal de cada usuario y envíales una notificación cuando lleguen a los 180megabytes de uso.  
  
-   No dejes almacenar archivos de música en las carpetas personales compartidas.  
  
-   Programa que todos los domingos a medianoche se ejecute un informe que genere una lista de los últimos archivos a los que se ha accedido en los dos últimos días. Este informe puede servir de ayuda para conocer la actividad de almacenamiento durante el fin de semana y planear el tiempo de inactividad del servidor según esa información.  

## <a name="see-also"></a>Consulta también

- [Novedades del Administrador de recursos del servidor de archivos](https://technet.microsoft.com/library/dn383587.aspx)
- [Control de acceso dinámico](https://technet.microsoft.com/library/dn408191(v=ws.11).aspx) 