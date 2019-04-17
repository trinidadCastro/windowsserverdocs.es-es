---
title: Antes de instalar Windows Server Essentials
description: "Describe cómo usar Windows Server Essentials"
ms.custom: na
ms.date: 10/03/2016
ms.prod: windows-server-2016-essentials
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 8d0893bd-e2b7-4494-9537-02b1cbbcd57a
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: e7eb1b7bed780b41f1a87589add4ab015f41624a
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/03/2017
---
# <a name="before-you-install-windows-server-essentials"></a>Antes de instalar Windows Server Essentials

>Se aplica a: Windows Server 2016 Essentials, Windows Server 2012 R2 Essentials, Windows Server 2012 Essentials

##  <a name="BKMK_BeforeYouBegin"></a>Antes de comenzar la instalación de Windows Server Essentials, realizar las siguientes tareas:  

-   **Asegúrate de que tu equipo cumple los requisitos mínimos de hardware**. Esto incluye determinar si es necesario hardware adicional y comprobar que los controladores para el hardware son compatibles con Windows Server Essentials. Para obtener más información, consulta [requisitos del sistema para Windows Server Essentials](../get-started/system-requirements.md).   

  
    > [!IMPORTANT]
    >  Antes de instalar Windows Server Essentials en un equipo ya existente, te recomendamos que completamente dar formato y, a continuación, volver a particionar los discos duros del equipo ya existente. Al formatear y volver a particionar los discos duros, quitar la posibilidad de que permanecen ocultas particiones en los discos duros.  
  
-   **Preparar la red** para preparar la red para instalar Windows Server Essentials, haz lo siguiente:  
    
  
    -   **Actualizar el sistema operativo en los equipos cliente** Windows Server Essentials admite los siguientes sistemas operativos: Windows 8, Windows 7, Windows 10 y Macintosh OS X Lion o superior. Estos sistemas operativos proporcionan la funcionalidad de la red local, el rendimiento, la confiabilidad y la características de seguridad necesario.  
  
    -   **Configurar el enrutador** comprobar que el enrutador está configurado como sigue:  
  
        -   El marco UPnP está habilitado en el enrutador.  
  
        -   El servicio de servidor de protocolo de configuración dinámica de Host (DHCP) para la LAN puede estar habilitado o deshabilitado.  ¿Windows Server Essentials garantiza que DHCP no se está ejecutando en el servidor y el enrutador? Cuando DHCP está habilitado en el enrutador, DHCP no está habilitado en el servidor durante la instalación.  
  
        -   Tienes una dirección IP para la interfaz externa del enrutador, que es proporcionada por el proveedor de servicios de Internet (ISP). El servicio de servidor DHCP en el ISP puede asignarse dinámicamente la dirección IP o debe configurar manualmente una dirección IP estática mediante la consola de administración del enrutador.  
  
        -   Si la conexión a Internet requiere un nombre de usuario y contraseña, también denominada Protocolo punto a punto en Ethernet (PPPoE), estas opciones se configuran en el enrutador, incluso si el dispositivo admite el marco UPnP.  
  
        -   El enrutador está conectado a la LAN y a Internet, se activa y funciona correctamente.  
  
     Si el enrutador no admite el marco UPnP, o si el enrutador no se puede configurar durante la instalación, debes configurarlo manualmente con la configuración de la red. Asegúrate de que los siguientes puertos admitirán y se dirigen a la dirección IP del servidor de destino:  
  
    |Número de puerto|Aplicación|  
    |-----------------|-----------------|  
    |Puerto 80|Tráfico HTTP Web|  
    |Puerto 443|Tráfico Web HTTPS|  
  

-   **Leer la documentación de la versión de la Windows Server Essentials**. La documentación de la versión contiene la información más reciente que puede ser esencial para instalar y configurar Windows Server Essentials correctamente. Para ver o imprimir la documentación de la versión, consulta [publicar documentación para Windows Server Essentials](../get-started/release-notes.md).  
  
## <a name="see-also"></a>Consulta también  
  
-   [Instalar Windows Server Essentials](Install-Windows-Server-Essentials.md)

