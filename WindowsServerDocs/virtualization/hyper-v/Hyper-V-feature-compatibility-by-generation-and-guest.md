---
title: Compatibilidad de las características de Hyper-V por generación e invitado
description: Enumera las generaciones y los sistemas operativos que son compatibles con las características de Hyper-V clave
manager: dongill
ms.topic: article
ms.assetid: 81c1f32d-7814-4992-8a66-dd4b77c939b4
author: kbdazure
ms.author: kathydav
ms.date: 12/05/2016
ms.openlocfilehash: 1ea9e0d86e61f574af45b85701bae941bb9f6ad5
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/07/2020
ms.locfileid: "87960758"
---
# <a name="hyper-v-feature-compatibility-by-generation-and-guest"></a>Compatibilidad de las características de Hyper-V por generación e invitado

>Se aplica a: Windows Server 2016

En las tablas de este artículo se muestran las generaciones y los sistemas operativos que son compatibles con algunas de las características de Hyper-V, agrupadas por categorías. En general, obtendrá la mejor disponibilidad de las características con una máquina virtual de generación 2 que ejecute el sistema operativo más reciente.

Tenga en cuenta que algunas características se basan en el hardware o en otra infraestructura. Para obtener información sobre el hardware, consulte [requisitos del sistema para Hyper-V en Windows Server 2016](System-requirements-for-Hyper-V-on-Windows.md). En algunos casos, una característica se puede usar con cualquier sistema operativo invitado compatible. Para obtener información detallada sobre qué sistemas operativos se admiten, consulte:

* [Máquinas virtuales Linux y FreeBSD compatibles](Supported-Linux-and-FreeBSD-virtual-machines-for-Hyper-V-on-Windows.md)
* [Sistemas operativos invitados Windows compatibles](Supported-Windows-guest-operating-systems-for-Hyper-V-on-Windows.md)

## <a name="availability-and-backup"></a>Disponibilidad y copia de seguridad

Característica  | Generation | Sistema operativo invitado
------------- | ------------- | -----------
Puntos de control | 1 y 2 | Cualquier invitado compatible
Agrupación en clústeres invitados | 1 y 2 | Invitados que ejecutan aplicaciones compatibles con clústeres y tienen instalado el software de destino iSCSI
Replicación | 1 y 2 | Cualquier invitado compatible
Controlador de dominio | 1 y 2 | Cualquier invitado de Windows Server compatible con solo puntos de control de producción. Consulte [sistemas operativos invitados compatibles con Windows Server](https://docs.microsoft.com/windows-server/virtualization/hyper-v/supported-windows-guest-operating-systems-for-hyper-v-on-windows#supported-windows-server-guest-operating-systems)

## <a name="compute"></a>Proceso

Característica  | Generation | Sistema operativo invitado
------------- | ------------- | -----------
Memoria dinámica | 1 y 2 | Versiones específicas de invitados admitidos. Vea [información general de memoria dinámica de Hyper-V](https://technet.microsoft.com/library/hh831766.aspx) para versiones anteriores a windows Server 2016 y Windows 10.
Adición o eliminación de memoria en caliente | 1 y 2 | Windows Server 2016, Windows 10
NUMA virtual | 1 y 2 | Cualquier invitado compatible

## <a name="development-and-test"></a>Desarrollo y pruebas
Característica  | Generation | Sistema operativo invitado
------------- | ------------- | -----------
Puertos COM/serie | 1 y 2 <br>**Nota:** Para la generación 2, use Windows PowerShell para configurar. Para obtener más información, vea [Agregar un puerto com para la depuración del kernel](./plan/should-i-create-a-generation-1-or-2-virtual-machine-in-hyper-v.md#add-a-com-port-for-kernel-debugging). | Cualquier invitado compatible

## <a name="mobility"></a>Movilidad

Característica  | Generation | Sistema operativo invitado
------------- | ------------- | -----------
Migración en vivo  | 1 y 2 |  Cualquier invitado compatible
Import/Export | 1 y 2 |  Cualquier invitado compatible

## <a name="networking"></a>Funciones de red

Característica  | Generation | Sistema operativo invitado
------------- | ------------- | -----------
Adición o eliminación en caliente del adaptador de red virtual | 2 | Cualquier invitado compatible
Adaptador de red virtual heredado | 1 | Cualquier invitado compatible
Virtualización de entrada/salida de raíz única (SR-IOV) | 1 y 2 | invitados de Windows de 64 bits, a partir de Windows Server 2012 y Windows 8.
Varias colas de máquinas virtuales (VMMQ) | 1 y 2  | Cualquier invitado compatible

## <a name="remote-connection-experience"></a>Experiencia de conexión remota

Característica  | Generation | Sistema operativo invitado
------------- | ------------- | -----------
Asignación discreta de dispositivos (DDA) | 1 y 2 | Windows Server 2016, Windows Server 2012 R2 solo con Update 3133690 instalado, Windows 10 <br> **Nota:** Para obtener más información sobre la actualización 3133690, consulte [este](https://support.microsoft.com/kb/3133690) artículo de soporte técnico.
Modo de sesión mejorada | 1 y 2 | Windows Server 2016, Windows Server 2012 R2, Windows 10 y Windows 8.1, con Servicios de Escritorio remoto habilitado <br>**Nota**: es posible que también tenga que configurar el host. Para obtener más información, consulte [uso de recursos locales en una máquina virtual de Hyper-V con VMConnect](./learn-more/Use-local-resources-on-Hyper-V-virtual-machine-with-VMConnect.md).
RemoteFx | 1 y 2 | Generación 1 en versiones de Windows de 32 bits y de 64 bits a partir de Windows 8. <br> Generación 2 en versiones de Windows 10 de 64 bits

## <a name="security"></a>Seguridad

Característica  | Generation | Sistema operativo invitado
------------- | ------------- | -----------
Arranque seguro | 2 | **Linux**: Ubuntu 14,04 y versiones posteriores, SUSE Linux Enterprise Server 12 y versiones posteriores, Red Hat Enterprise Linux 7,0 y versiones posteriores, y versión 7,0 y versiones posteriores<br>**Windows**: todas las versiones compatibles que se pueden ejecutar en una máquina virtual de generación 2
Máquinas virtuales blindadas | 2 | **Windows**: todas las versiones compatibles que se pueden ejecutar en una máquina virtual de generación 2

## <a name="storage"></a>Almacenamiento

Característica  | Generation | Sistema operativo invitado
------------- | ------------- | -----------
Discos duros virtuales compartidos (solo VHDX) | 1 y 2  | Windows Server 2016, Windows Server 2012 R2, Windows Server 2012
SMB3 | 1 y 2 | Todo eso es compatible con SMB3
Espacios de almacenamiento directo | 2 | Windows Server 2016
Canal de fibra virtual | 1 y 2 | Windows Server 2016, Windows Server 2012 R2, Windows Server 2012
Formato VHDX | 1 y 2 | Cualquier invitado compatible







