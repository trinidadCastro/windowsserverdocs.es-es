---
title: 'Notas de la versión: problemas importantes de Windows Server, versión 1709'
description: Se resumen problemas críticos que requieren soluciones para evitar bloqueos, faltas de respuesta, errores de instalación o pérdida de datos.
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.date: 04/23/2018
ms.technology: server-general
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 134aab85-664f-4d44-87ef-9e5fd389071f
author: jaimeo
ms.author: jaimeo
manager: dougkim
ms.localizationpriority: medium
ms.openlocfilehash: 4eebc498289a81c7f27fcf4b84d81ae13bc38e4f
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59861986"
---
# <a name="release-notes-important-issues-in-windows-server-version-1709"></a>Notas de la versión: Problemas importantes en Windows Server, versión 1709

>Se aplica a: Canal semianual de Windows Server

En estas notas se resumen los problemas más críticos del sistema operativo Windows Server&reg; junto con soluciones alternativas y formas de evitar los problemas, si se conocen. Para más información sobre los cambios por diseño, nuevas características y soluciones en esta versión, consulta [Novedades de Windows Server, versión 1709](whats-new-in-windows-server-1709.md) y anuncios de equipos de características específicas. A menos que se especifique lo contrario, cada problema notificado se aplica a todas las ediciones y opciones de instalación de Windows Server 2016.  

Este documento se actualiza continuamente. Habida cuenta de que se detectan problemas críticos que necesitan una solución alternativa, se agregan, al igual que las nuevas soluciones alternativas y correcciones, a medida que están disponibles.  
  
## <a name="storage-spaces-direct"></a>Espacios de almacenamiento directo
[comment]: # (Id.: desconocido; Remitente: stevenek; estado: firmado)  
Espacios de almacenamiento directo no se incluye en Windows Server, versión 1709. Si llamas a *Enable-ClusterStorageSpacesDirect* o a su alias *Enable-ClusterS2D* en un servidor que ejecuta Windows Server, versión 1709, recibirás un error con el mensaje "No se admite la operación solicitada".

Tampoco es compatible para presentar servidores que ejecutan Windows Server, versión 1709 en una implementación de Espacios de almacenamiento directos de Windows Server 2016.

El modelo de lanzamiento de Windows Server ofrece una nueva opción para alinearse con modelos de mantenimiento y lanzamiento similares para [Windows 10](https://docs.microsoft.com/windows/deployment/update/waas-overview) y [Office 365 ProPlus](https://support.office.com/article/Overview-of-the-upcoming-changes-to-Office-365-ProPlus-update-management-78b33779-9356-4cdf-9d2c-08350ef05cca?ui=en-US&rs=en-US&ad=US). Los lanzamientos del Canal semianual ofrecen nuevas funciones para los clientes que quieren mover a un ritmo rápido y tendrán novedades disponibles dos veces al año, en primavera y en otoño.

El canal semianual de Windows Server se centra en escenarios de aplicaciones que se beneficien de una innovación más rápida y contenedores, consulte esta [blog](https://cloudblogs.microsoft.com/windowsserver/2018/03/29/windows-server-semi-annual-channel-update) para obtener más información. Los clientes que deseen para roles de infraestructura, como espacios de almacenamiento directo, deben usar las versiones de canal de servicio a largo plazo, como Windows Server 2016 (ya está disponible) y [Windows Server 2019](https://cloudblogs.microsoft.com/windowsserver/2018/03/20/introducing-windows-server-2019-now-available-in-preview) (que se lanzarán este año). Estamos comprometidos a crear la mejor plataforma para infraestructura hiperconvergente y seguimos desarrollar nuevas características y mejorar las existentes según sus comentarios. 

Espacios de almacenamiento directos se presentó en Windows Server 2016 y es la base de nuestra plataforma hiperconvergida. Estamos encantados con la adopción positiva de la plataforma hiperconvergida de Microsoft y nos comprometemos con nuestros clientes.

Se ha escuchado sus comentarios y estamos trabajando para ofrecer la [siguiente conjunto de innovaciones](https://blogs.technet.microsoft.com/windowsserver/2017/09/07/sneak-peek-2-windows-server-version-1709-hyper-converged-infrastructure/) para nuestra plataforma hiperconvergido. Estas características están disponibles hoy día en [Windows Insiders](https://insider.windows.com/for-business/) compilaciones y nos encantaría para que probarlas y compartir sus comentarios. Para aquellos clientes que buscan una solución hiperconvergida validada, recomendamos el programa de [Windows Server definido por software](http://microsoft.com/wssd).
