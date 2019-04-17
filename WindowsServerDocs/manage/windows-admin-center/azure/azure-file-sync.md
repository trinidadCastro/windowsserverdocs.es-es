---
title: Sincronizar el servidor de archivos con la nube mediante el uso de sincronización de archivos de Azure
description: Usa la sincronización de archivos de Azure para centralizar recursos compartidos de archivos de la organización en Azure, manteniendo la flexibilidad, rendimiento y compatibilidad de un servidor de archivos local. Sincronización de Azure archivo transforma Windows Server en una memoria caché rápida del recurso compartido de archivo Azure con la característica de niveles opcionales en la nube.
ms.technology: manage
ms.topic: article
author: fauhse
ms.author: fauhse
ms.date: 04/12/2019
ms.localizationpriority: medium
ms.prod: windows-server-threshold
ms.openlocfilehash: 93cfa469a3c14410d95b0b224cbf59b913ec9f7e
ms.sourcegitcommit: f1edfc6525e09dd116b106293f9260123a94de0c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/12/2019
ms.locfileid: "9297093"
---
# Sincronizar el servidor de archivos con la nube mediante el uso de sincronización de archivos de Azure

>Se aplica a: Windows Admin Center, versión preliminar de Windows Admin Center

Usa la sincronización de archivos de Azure para centralizar recursos compartidos de archivos de la organización en Azure, manteniendo la flexibilidad, rendimiento y compatibilidad de un servidor de archivos local. Sincronización de Azure archivo transforma Windows Server en una memoria caché rápida del recurso compartido de archivo Azure con la característica de niveles opcionales en la nube. Puedes usar cualquier protocolo que está disponible en Windows Server para acceder a los datos de forma local, incluidos SMB, NFS y FTPS.

Una vez que se han sincronizado los archivos en la nube, puedes conectar varios servidores para el mismo recurso compartido de archivo de Azure para sincronizar y almacenar en caché el contenido localmente, permisos (ACL) siempre se transportan también. Azure Files ofrece una funcionalidad de instantánea que puede generar instantáneas diferenciales del recurso compartido de archivo de Azure. Estas instantáneas se pueden montar incluso como unidades de red de solo lectura a través de SMB para la exploración fácil y la restauración. En combinación con niveles de nube, ejecuta un servidor de archivos locales nunca ha sido más fácil.

Para obtener más información, vea [Planear una implementación de sincronización de archivos de Azure](https://aka.ms/afs).