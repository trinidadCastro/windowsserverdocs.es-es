---
title: Sincronizar el servidor de archivos con la nube mediante Azure File Sync
description: Use Azure File Sync para centralizar los recursos compartidos de archivos de su organización en Azure, a la vez que mantiene la flexibilidad, el rendimiento y la compatibilidad de un servidor de archivos local. Azure File Sync transforma Windows Server en una caché rápida de su recurso compartido de archivos de Azure con la característica de niveles de nube opcional.
ms.technology: manage
ms.topic: article
author: fauhse
ms.author: fauhse
ms.date: 04/12/2019
ms.localizationpriority: medium
ms.prod: windows-server
ms.openlocfilehash: fe0e3337962b7d9c2f025a9d4eba826f3349c1f9
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71357376"
---
# <a name="sync-your-file-server-with-the-cloud-by-using-azure-file-sync"></a>Sincronizar el servidor de archivos con la nube mediante Azure File Sync

>Se aplica a: Windows Admin Center, versión preliminar de Windows Admin Center

Use Azure File Sync para centralizar los recursos compartidos de archivos de su organización en Azure, a la vez que mantiene la flexibilidad, el rendimiento y la compatibilidad de un servidor de archivos local. Azure File Sync transforma Windows Server en una caché rápida de su recurso compartido de archivos de Azure con la característica de niveles de nube opcional. Puede usar cualquier protocolo que esté disponible en Windows Server para tener acceso a los datos localmente, como SMB, NFS y FTPS.

Una vez que los archivos se han sincronizado en la nube, puede conectar varios servidores al mismo recurso compartido de archivos de Azure para sincronizar y almacenar en caché el contenido localmente; los permisos (ACL) siempre se transportan. Azure Files ofrece una funcionalidad de instantánea que puede generar instantáneas diferenciales del recurso compartido de archivos de Azure. Estas instantáneas se pueden montar incluso como unidades de red de solo lectura a través de SMB para facilitar la exploración y la restauración. En combinación con los niveles en la nube, la ejecución de un servidor de archivos local nunca ha sido tan fácil.

Para obtener más información, consulte [planeación de una implementación de Azure File Sync](https://aka.ms/afs).