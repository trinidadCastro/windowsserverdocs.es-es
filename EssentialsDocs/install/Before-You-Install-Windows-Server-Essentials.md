---
title: Antes de instalar Windows Server Essentials
description: Describe cómo usar Windows Server Essentials
ms.date: 10/03/2016
ms.topic: article
ms.assetid: 8d0893bd-e2b7-4494-9537-02b1cbbcd57a
author: nnamuhcs
ms.author: geschuma
manager: mtillman
ms.openlocfilehash: ceb5d81490b46513b540901413d8923d61d87e40
ms.sourcegitcommit: db2d46842c68813d043738d6523f13d8454fc972
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/10/2020
ms.locfileid: "89623941"
---
# <a name="before-you-install-windows-server-essentials"></a>Antes de instalar Windows Server Essentials

>Se aplica a: Windows Server 2016 Essentials, Windows Server 2012 R2 Essentials, Windows Server 2012 Essentials

##  <a name="before-you-begin-your-installation-of--windows-server-essentials-perform-the-following-tasks"></a><a name="BKMK_BeforeYouBegin"></a> Antes de comenzar la instalación de Windows Server Essentials, realice las siguientes tareas:

-   **Asegúrese de que su equipo cumpla los requisitos mínimos de hardware**. Esto incluye determinar si necesita hardware adicional y comprobar que los controladores del hardware son compatibles con Windows Server Essentials. Para obtener más información, vea [requisitos del sistema para Windows Server Essentials](../get-started/system-requirements.md).

> [!IMPORTANT]
> Antes de instalar Windows Server Essentials en un equipo preexistente, se recomienda formatear completamente y volver a particionar los discos duros del equipo existente previamente. De esta manera, se elimina la posibilidad de que permanezcan particiones ocultas en los discos duros.

- **Preparar la red** Para preparar la red para instalar Windows Server Essentials, haga lo siguiente:


  - **Actualizar el sistema operativo en los equipos cliente**  Windows Server Essentials es compatible con los siguientes sistemas operativos: Windows 8, Windows 7, Windows 10 y Macintosh OS X Lion o superior. Estos sistemas operativos proporcionan las características de seguridad, la confiabilidad, el rendimiento y la funcionalidad que son necesarios para la red local.

  - **Configure el enrutador** Compruebe que el enrutador esté configurado de la manera siguiente:

    -   El entorno UPnP está habilitado en el enrutador.

    -   El servicio del servidor del Protocolo de configuración dinámica de host (DHCP) de la LAN puede estar habilitado o deshabilitado.  Windows Server Essentials garantiza que DHCP no se está ejecutando tanto en el servidor como en el enrutador. cuando DHCP está habilitado en el enrutador, DHCP no está habilitado en el servidor durante la instalación.

    -   Tiene una dirección IP para la interfaz externa del enrutador, que le ha suministrado el proveedor de acceso a Internet (ISP). La dirección IP puede asignarla dinámicamente el servicio del servidor DHCP en el ISP, o usted deberá configurar manualmente una dirección IP estática mediante la consola de administración del enrutador.

    -   Si su conexión a Internet precisa de un nombre de usuario y una contraseña, lo que se conoce también como Protocolo punto a punto en Ethernet (PPPoE), estos valores están configurados en el enrutador, incluso si el dispositivo admite el entorno UPnP.

    -   El enrutador está conectado a la LAN y a Internet, está encendido y funciona correctamente.

    Si el enrutador no es compatible con el entorno UPnP o no se puede configurar durante la instalación, deberá configurarlo manualmente con los valores de la red. Asegúrese de que los siguientes puertos estén abiertos y de que estén dirigidos a la dirección IP del servidor de destino:

  |Número de puerto|Application|
  |-----------------|-----------------|
  |Puerto 80|Tráfico web HTTP|
  |Puerto 443|Tráfico web HTTPS|


- **Lea la documentación de la versión de Windows Server Essentials**. La documentación de la versión contiene la información más reciente que puede resultar fundamental para instalar y configurar correctamente Windows Server Essentials. Para ver o imprimir la documentación de la versión, consulte la [documentación de la versión de Windows Server Essentials](../get-started/release-notes.md).

## <a name="additional-references"></a>Referencias adicionales

-   [Instalación de Windows Server 2012 Essentials](Install-Windows-Server-Essentials.md)

