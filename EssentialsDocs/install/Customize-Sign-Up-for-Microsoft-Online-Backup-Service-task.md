---
title: Personalizar la tarea Suscribirse a Microsoft Online Backup Service
description: Describe cómo usar Windows Server Essentials
ms.custom: na
ms.date: 10/03/2016
ms.prod: windows-server-2016-essentials
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: a7eafbb3-7728-487e-b287-90bbd6fee7f0
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: cd148e0e58cd80dbff7f7884ead95dc1e46b6257
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59879936"
---
# <a name="customize-sign-up-for-microsoft-online-backup-service-task"></a>Personalizar la tarea Suscribirse a Microsoft Online Backup Service

>Se aplica a: Windows Server 2016 Essentials, Windows Server 2012 R2 Essentials, Windows Server 2012 Essentials

De forma predeterminada, la tarea **Suscribirse a Microsoft Online Backup Service** en la pestaña **DISPOSITIVOS** del panel abre el sitio web de Microsoft Online Backup Service. El sitio web proporciona información sobre el servicio y le ayuda a suscribirse al servicio y a descargar el software necesario.  
  
 La tarea **Suscribirse a Microsoft Online Backup Service** se puede personalizar de dos formas:  
  
-   Puede sustituir la URL del sitio web predeterminado por una URL que represente una experiencia del cliente personalizada. Para reemplazar la dirección URL predeterminada, abra el Editor del registro, cree la clave del Registro: **HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows Server\OnlineBackup\LinkUrl** y después asigne la dirección URL personalizada como valor de la clave.  
  
-   Puede ocultar la tarea: Para ocultar la tarea, abra el Editor del registro y cree la clave del Registro: **HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows Server\OnlineBackup\OnlineBackupInstalled**.  
  
## <a name="see-also"></a>Vea también  
 [Crear y personalizar la imagen](Creating-and-Customizing-the-Image.md)   
 [Personalizaciones adicionales](Additional-Customizations.md)   
 [Preparar la imagen para la implementación](Preparing-the-Image-for-Deployment.md)   
 [Probar la experiencia del cliente](Testing-the-Customer-Experience.md)