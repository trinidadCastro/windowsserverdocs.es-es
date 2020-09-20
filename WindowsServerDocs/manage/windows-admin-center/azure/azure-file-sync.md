---
title: Sincroniza tu servidor de archivos con la nube mediante Azure File Sync
description: Use Azure File Sync para centralizar los recursos compartidos de archivos de su organización en Azure, a la vez que mantiene la flexibilidad, el rendimiento y la compatibilidad de un servidor de archivos local. Azure File Sync transforma Windows Server en una caché rápida de su recurso compartido de archivos de Azure con la característica de niveles de nube opcional.
ms.topic: article
author: fauhse
ms.author: fauhse
ms.date: 04/12/2019
ms.localizationpriority: medium
ms.openlocfilehash: 56937ad0351a1421ab64b93351d7fa5f6d9f4fe8
ms.sourcegitcommit: 5344adcf9c0462561a4f9d47d80afc1d095a5b13
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/18/2020
ms.locfileid: "90766688"
---
# <a name="sync-your-file-server-with-the-cloud-by-using-azure-file-sync"></a>Sincroniza tu servidor de archivos con la nube mediante Azure File Sync

>Se aplica a: Windows Admin Center, versión preliminar de Windows Admin Center

Use Azure File Sync para centralizar los recursos compartidos de archivos de su organización en Azure, a la vez que mantiene la flexibilidad, el rendimiento y la compatibilidad de un servidor de archivos local. Azure File Sync transforma Windows Server en una caché rápida de su recurso compartido de archivos de Azure con la característica de niveles de nube opcional. Puede usar cualquier protocolo disponible en Windows Server para acceder a sus datos localmente, como SMB, NFS y FTPS.

Una vez que los archivos se han sincronizado en la nube, puede conectar varios servidores al mismo recurso compartido de archivos de Azure para sincronizar y almacenar en caché el contenido localmente; los permisos (ACL) siempre se transportan. Azure Files ofrece una funcionalidad de instantánea que puede generar instantáneas diferenciales del recurso compartido de archivos de Azure. Estas instantáneas se pueden montar incluso como unidades de red de solo lectura a través de SMB para facilitar la exploración y la restauración. En combinación con los niveles en la nube, la ejecución de un servidor de archivos local nunca ha sido tan fácil.

Para obtener más información, consulte [planeación de una implementación de Azure File Sync](/azure/storage/files/storage-sync-files-planning).