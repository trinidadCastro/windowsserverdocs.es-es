---
title: Antes de instalar Windows Server Essentials
description: Describe cómo usar Windows Server Essentials
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
ms.openlocfilehash: 32b2b37e0d0109b8ad2a991b9f7693139103734d
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/31/2019
ms.locfileid: "66433714"
---
# <a name="before-you-install-windows-server-essentials"></a>Antes de instalar Windows Server Essentials

>Se aplica a: Windows Server 2016 Essentials, Windows Server 2012 R2 Essentials, Windows Server 2012 Essentials

##  <a name="BKMK_BeforeYouBegin"></a> Antes de comenzar la instalación de Windows Server Essentials, realice las siguientes tareas:  

-   **Asegúrese de que su equipo cumpla los requisitos mínimos de hardware**. Esto incluye determinar si necesita hardware adicional y comprobar que los controladores del hardware son compatibles con Windows Server Essentials. Para obtener más información, consulte [requisitos del sistema para Windows Server Essentials](../get-started/system-requirements.md).   


~~~
> [!IMPORTANT]
>  Before you install  Windows Server Essentials on a pre-existing computer, we recommend that you fully format and then repartition the hard disks of the pre-existing computer. By formatting and repartitioning the hard disks, you remove the possibility that hidden partitions remain on the hard disks.  
~~~

- **Preparar la red** para preparar la red para instalar Windows Server Essentials, haga lo siguiente:  


  - **Actualizar sistema operativo en los equipos cliente** Windows Server Essentials admite los siguientes sistemas operativos:  Windows 8, Windows 7, Windows 10 y Macintosh OS X Lion o superior. Estos sistemas operativos proporcionan las características de seguridad, la confiabilidad, el rendimiento y la funcionalidad que son necesarios para la red local.  

  - **Configure el enrutador** Compruebe que el enrutador esté configurado de la manera siguiente:  

    -   El entorno UPnP está habilitado en el enrutador.  

    -   El servicio del servidor del Protocolo de configuración dinámica de host (DHCP) de la LAN puede estar habilitado o deshabilitado.  ¿Windows Server Essentials garantiza que DHCP no se está ejecutando en el servidor y el enrutador? Si se habilita en el enrutador, DHCP no está habilitado en el servidor durante la instalación.  

    -   Tiene una dirección IP para la interfaz externa del enrutador, que le ha suministrado el proveedor de acceso a Internet (ISP). La dirección IP puede asignarla dinámicamente el servicio del servidor DHCP en el ISP, o usted deberá configurar manualmente una dirección IP estática mediante la consola de administración del enrutador.  

    -   Si su conexión a Internet precisa de un nombre de usuario y una contraseña, lo que se conoce también como Protocolo punto a punto en Ethernet (PPPoE), estos valores están configurados en el enrutador, incluso si el dispositivo admite el entorno UPnP.  

    -   El enrutador está conectado a la LAN y a Internet, está encendido y funciona correctamente.  

    Si el enrutador no es compatible con el entorno UPnP o no se puede configurar durante la instalación, deberá configurarlo manualmente con los valores de la red. Asegúrese de que los siguientes puertos estén abiertos y de que estén dirigidos a la dirección IP del servidor de destino:  

  |Número de puerto|Application|  
  |-----------------|-----------------|  
  |Puerto 80|Tráfico web HTTP|  
  |Puerto 443|Tráfico web HTTPS|  


- **Lea la documentación de la versión de Windows Server Essentials**. La documentación de la versión contiene la información más reciente que puede resultar fundamental para instalar y configurar Windows Server Essentials correctamente. Para ver o imprimir la documentación de la versión, consulte [Release Documentation for Windows Server Essentials](../get-started/release-notes.md).  

## <a name="see-also"></a>Vea también  

-   [Instalación de Windows Server 2012 Essentials](Install-Windows-Server-Essentials.md)

