---
title: Compatibilidad de la característica Hyper-V mediante la generación e invitado
description: Enumera las generaciones y sistemas operativos que son compatibles con las características clave de Hyper-V
ms.prod: windows-server-threshold
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 81c1f32d-7814-4992-8a66-dd4b77c939b4
author: KBDAzure
ms.author: kathydav
ms.date: 12/05/2016
ms.openlocfilehash: 1863c1736d3c8573b3d11c6bef492c6645d28a77
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59859766"
---
# <a name="hyper-v-feature-compatibility-by-generation-and-guest"></a>Compatibilidad de la característica Hyper-V mediante la generación e invitado

>Se aplica a: Windows Server 2016
  
Las tablas en este artículo muestran las generaciones y sistemas operativos que son compatibles con algunas de las características de Hyper-V, agrupadas por categorías. En general, obtendrá la mejor disponibilidad de características con una máquina virtual de generación 2 que se ejecuta el sistema operativo más reciente.  
  
Tenga en cuenta que algunas características dependen de hardware u otra infraestructura. Para obtener detalles de hardware, consulte [requisitos del sistema para Hyper-V en Windows Server 2016](System-requirements-for-Hyper-V-on-Windows.md). En algunos casos, una característica puede usarse con cualquier sistema operativo invitado admitido. Para obtener más información en el que se admiten sistemas operativos, consulte:  
  
* [De máquinas virtuales Linux y FreeBSD compatibles](Supported-Linux-and-FreeBSD-virtual-machines-for-Hyper-V-on-Windows.md)  
* [Sistemas de operativos de invitado de Windows](Supported-Windows-guest-operating-systems-for-Hyper-V-on-Windows.md)  
  
## <a name="availability-and-backup"></a>Copia de seguridad y disponibilidad  
  
Característica  | generación | Sistema operativo invitado  
------------- | ------------- | -----------  
Puntos de comprobación | 1 y 2 | Los invitados compatibles  
Clústeres invitados | 1 y 2 | Invitados que ejecutan aplicaciones compatibles con clústeres y tienen instalado el software de destino iSCSI  
Replicación | 1 y 2 | Los invitados compatibles  
Controlador de dominio | 1 y 2 | Cualquier admitido con solo puntos de control de producción de invitado de Windows Server. Consulte [sistemas de operativos invitados compatibles con Windows Server](https://docs.microsoft.com/windows-server/virtualization/hyper-v/supported-windows-guest-operating-systems-for-hyper-v-on-windows#supported-windows-server-guest-operating-systems)   
  
## <a name="compute"></a>Cálculo  
  
Característica  | generación | Sistema operativo invitado  
------------- | ------------- | -----------  
Memoria dinámica | 1 y 2 | Versiones específicas de los invitados compatibles. Consulte [Hyper-V Dynamic Memory Overview](https://technet.microsoft.com/library/hh831766.aspx) para las versiones anteriores a Windows Server 2016 y Windows 10.  
Agregar/extracción en caliente de memoria | 1 y 2 | Windows Server 2016, Windows 10  
NUMA virtual | 1 y 2 | Los invitados compatibles  
  
## <a name="development-and-test"></a>Desarrollo y pruebas  
Característica  | generación | Sistema operativo invitado  
------------- | ------------- | -----------  
Puertos COM/serie | 1 y 2 <br>**Nota:** Para la generación 2, use Windows PowerShell para configurar. Para obtener más información, consulte [agregar un puerto COM para la depuración de kernel](./plan/should-i-create-a-generation-1-or-2-virtual-machine-in-hyper-v.md#BKMK_Debug). | Los invitados compatibles  
  
## <a name="mobility"></a>Movilidad  
  
Característica  | generación | Sistema operativo invitado  
------------- | ------------- | -----------  
Migración en vivo  | 1 y 2 |  Los invitados compatibles  
Import/export | 1 y 2 |  Los invitados compatibles  
  
## <a name="networking"></a>Funciones de red  
  
Característica  | generación | Sistema operativo invitado  
------------- | ------------- | -----------  
Agregar/extracción en caliente de adaptador de red virtual | 2 | Los invitados compatibles  
Adaptador de red virtual heredado | 1 | Los invitados compatibles  
Virtualización de entrada/salida de raíz única (SR-IOV) | 1 y 2 | invitados de Windows de 64 bits, a partir de Windows Server 2012 y Windows 8.  
Cola de la máquina virtual con varias (VMMQ) | 1 y 2  | Los invitados compatibles  
  
## <a name="remote-connection-experience"></a>Experiencia de conexión remota  
  
Característica  | generación | Sistema operativo invitado  
------------- | ------------- | -----------  
Asignación discreta de dispositivos (DDA) | 1 y 2 | Windows Server 2016, Windows Server 2012 R2 sólo con actualización 3133690 instalada, Windows 10 <br> **Nota:** Para más información sobre la actualización 3133690, consulte [esto](https://support.microsoft.com/kb/3133690) artículo de soporte técnico.  
Modo de sesión mejorada | 1 y 2 | Windows Server 2016, Windows Server 2012 R2, Windows 10 y Windows 8.1, con servicios de escritorio remoto habilitado <br>**Nota**: Es posible que deba también configurar el host. Para obtener más información, consulte [usar los recursos locales en la máquina virtual de Hyper-V con VMConnect](./learn-more/Use-local-resources-on-Hyper-V-virtual-machine-with-VMConnect.md).  
RemoteFx | 1 y 2 | Generación 1 en versiones de Windows de 32 bits y 64 bits a partir de Windows 8. <br> Generación 2 en las versiones de 64 bits de Windows 10  
  
## <a name="security"></a>Seguridad  
  
Característica  | generación | Sistema operativo invitado  
------------- | ------------- | -----------  
Arranque seguro | 2 | **Linux**: Ubuntu 14.04 y versiones posterior, SUSE Linux Enterprise Server 12 y versiones posterior, rojo Hat Enterprise Linux 7.0 y versiones posterior y CentOS 7.0 y versiones posteriores<br>**Windows**: Todas las versiones admitidas que se pueden ejecutar en una máquina virtual de generación 2  
Máquinas virtuales blindadas | 2 | **Windows**: Todas las versiones admitidas que se pueden ejecutar en una máquina virtual de generación 2  
  
## <a name="storage"></a>Almacenamiento  
  
Característica  | generación | Sistema operativo invitado  
------------- | ------------- | -----------  
Discos duros virtuales (sólo VHDX) compartidos | 1 y 2  | Windows Server 2016, Windows Server 2012 R2, Windows Server 2012  
SMB3 | 1 y 2 | Todo lo que admiten SMB3  
Espacios de almacenamiento directo | 2 | Windows Server 2016  
Canal de fibra virtual | 1 y 2 | Windows Server 2016, Windows Server 2012 R2, Windows Server 2012  
Formato VHDX | 1 y 2 | Los invitados compatibles   
  
  
  
  
    


