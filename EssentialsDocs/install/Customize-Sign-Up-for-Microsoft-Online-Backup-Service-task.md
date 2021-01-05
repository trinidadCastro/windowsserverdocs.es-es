---
title: Personalizar la tarea Suscribirse a Microsoft Online Backup Service
description: Obtenga información acerca de cómo personalizar la tarea suscribirse a Microsoft Online Backup Service.
ms.date: 10/03/2016
ms.topic: article
ms.assetid: a7eafbb3-7728-487e-b287-90bbd6fee7f0
author: nnamuhcs
ms.author: geschuma
manager: mtillman
ms.openlocfilehash: 3c34e948da79d5b6a21b24da575d8b5b23022d96
ms.sourcegitcommit: e00e789dff216dbade861e61365f078b758a5720
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/23/2020
ms.locfileid: "97755111"
---
# <a name="customize-sign-up-for-microsoft-online-backup-service-task"></a>Personalizar la tarea Suscribirse a Microsoft Online Backup Service

>Se aplica a: Windows Server 2016 Essentials, Windows Server 2012 R2 Essentials, Windows Server 2012 Essentials

De forma predeterminada, la tarea **Suscribirse a Microsoft Online Backup Service** en la pestaña **DISPOSITIVOS** del panel abre el sitio web de Microsoft Online Backup Service. El sitio web proporciona información sobre el servicio y le ayuda a suscribirse al servicio y a descargar el software necesario.

 La tarea **Suscribirse a Microsoft Online Backup Service** se puede personalizar de dos formas:

-   Puede sustituir la URL del sitio web predeterminado por una URL que represente una experiencia del cliente personalizada. Para sustituir la URL predeterminada, abra el Editor del Registro, cree la clave del Registro: **HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows Server\OnlineBackup\LinkUrl** y, a continuación, asigne la URL personalizada como valor de la clave.

-   Puede ocultar la tarea: Para ocultar la tarea, abra el Editor del Registro y cree la clave del Registro: **HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows Server\OnlineBackup\OnlineBackupInstalled**.

## <a name="see-also"></a>Consulte también
 [Crear y personalizar la imagen](Creating-and-Customizing-the-Image.md) [personalizaciones adicionales](Additional-Customizations.md) [preparar la imagen para probar la implementación de](Preparing-the-Image-for-Deployment.md) [la experiencia del cliente](Testing-the-Customer-Experience.md)