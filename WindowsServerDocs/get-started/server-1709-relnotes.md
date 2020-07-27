---
title: 'Notas de la versión: problemas importantes de Windows Server, versión 1709'
description: Se resumen problemas críticos que requieren soluciones para evitar bloqueos, faltas de respuesta, errores de instalación o pérdida de datos.
ms.prod: windows-server
ms.date: 04/23/2018
ms.technology: server-general
ms.topic: article
ms.assetid: 134aab85-664f-4d44-87ef-9e5fd389071f
author: jaimeo
ms.author: jaimeo
manager: dougkim
ms.localizationpriority: medium
ms.openlocfilehash: 64756756449b811d2f7fbb109ac93837567413d4
ms.sourcegitcommit: d5e27c1f2f168a71ae272bebf8f50e1b3ccbcca3
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/23/2020
ms.locfileid: "86955647"
---
# <a name="release-notes-important-issues-in-windows-server-version-1709"></a>Notas de la versión: Problemas importantes en Windows Server, versión 1709

>Se aplica a: Canal semianual de Windows Server

En estas notas se resumen los problemas más críticos del sistema operativo Windows Server&reg; junto con soluciones alternativas y formas de evitar los problemas, si se conocen. Para más información sobre los cambios por diseño, nuevas características y soluciones en esta versión, consulta [Novedades de Windows Server, versión 1709](whats-new-in-windows-server-1709.md) y anuncios de equipos de características específicas. A menos que se especifique lo contrario, cada problema notificado se aplica a todas las ediciones y opciones de instalación de Windows Server 2016.  

Este documento se actualiza continuamente. Habida cuenta de que se detectan problemas críticos que necesitan una solución alternativa, se agregan, al igual que las nuevas soluciones alternativas y correcciones, a medida que están disponibles.  
  
## <a name="storage-spaces-direct"></a>Espacios de almacenamiento directos
[comment]: # (Id.: desconocido; Remitente: stevenek; estado: aprobado)  
Espacios de almacenamiento directo no se incluye en Windows Server, versión 1709. Si llamas a *Enable-ClusterStorageSpacesDirect* o a su alias *Enable-ClusterS2D* en un servidor que ejecuta Windows Server, versión 1709, recibirás un error con el mensaje No se admite la operación solicitada.

Tampoco es compatible para presentar servidores que ejecutan Windows Server, versión 1709 en una implementación de Espacios de almacenamiento directo de Windows Server 2016.

El modelo de lanzamiento de Windows Server ofrece una nueva opción para alinearse con modelos de mantenimiento y lanzamiento similares para [Windows 10](/windows/deployment/update/waas-overview) y [Office 365 ProPlus](https://support.office.com/article/Overview-of-the-upcoming-changes-to-Office-365-ProPlus-update-management-78b33779-9356-4cdf-9d2c-08350ef05cca?ui=en-US&rs=en-US&ad=US). Los lanzamientos del Canal semianual ofrecen nuevas funciones para los clientes que quieren mover a un ritmo rápido y tendrán novedades disponibles dos veces al año, en primavera y en otoño.

El Canal semianual de Windows Server se centra en contenedores y escenarios de aplicación que se benefician de una innovación más rápida; consulta este [blog](https://cloudblogs.microsoft.com/windowsserver/2018/03/29/windows-server-semi-annual-channel-update) para más información. Los clientes que buscan roles de infraestructura, como Espacios de almacenamiento directo, deben usar versiones del Canal de mantenimiento a largo plazo como Windows Server 2016 (disponible ahora) y [Windows Server 2019](https://cloudblogs.microsoft.com/windowsserver/2018/03/20/introducing-windows-server-2019-now-available-in-preview) (que se publicará este año). Estamos comprometidos a crear la mejor plataforma para la infraestructura hiperconvergida y seguimos desarrollando nuevas características y mejorando las existentes según tus comentarios. 

Espacios de almacenamiento directo se presentó en Windows Server 2016 y es la base de nuestra plataforma hiperconvergida. Estamos encantados con la adopción positiva de la plataforma hiperconvergida de Microsoft y nos comprometemos con nuestros clientes.

Hemos escuchado vuestros comentarios y estamos trabajando para ofrecer el [próximo conjunto de innovaciones](https://cloudblogs.microsoft.com/windowsserver/2017/09/07/sneak-peek-2-windows-server-version-1709-hyper-converged-infrastructure/) para nuestra plataforma hiperconvergida. Estas características están disponibles actualmente en las compilaciones de [Windows Insider](https://insider.windows.com/for-business/) y nos encantaría que las probaras y compartieras tu opinión. Para aquellos clientes que buscan una solución hiperconvergida validada, recomendamos el programa de [Windows Server definido por software](https://microsoft.com/wssd).
