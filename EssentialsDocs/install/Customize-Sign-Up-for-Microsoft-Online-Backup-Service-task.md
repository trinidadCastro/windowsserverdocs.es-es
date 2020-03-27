---
title: Personalizar la tarea Suscribirse a Microsoft Online Backup Service
description: Describe cómo usar Windows Server Essentials
ms.custom: na
ms.date: 10/03/2016
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: a7eafbb3-7728-487e-b287-90bbd6fee7f0
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: f6e0a499fd149706dc3d7cce21935a7f8b5c5a48
ms.sourcegitcommit: da7b9bce1eba369bcd156639276f6899714e279f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/26/2020
ms.locfileid: "80311903"
---
# <a name="customize-sign-up-for-microsoft-online-backup-service-task"></a>Personalizar la tarea Suscribirse a Microsoft Online Backup Service

>Se aplica a: Windows Server 2016 Essentials, Windows Server 2012 R2 Essentials, Windows Server 2012 Essentials

De forma predeterminada, la tarea **Suscribirse a Microsoft Online Backup Service** en la pestaña **DISPOSITIVOS** del panel abre el sitio web de Microsoft Online Backup Service. El sitio web proporciona información sobre el servicio y le ayuda a suscribirse al servicio y a descargar el software necesario.  
  
 La tarea **Suscribirse a Microsoft Online Backup Service** se puede personalizar de dos formas:  
  
-   Puede sustituir la URL del sitio web predeterminado por una URL que represente una experiencia del cliente personalizada. Para sustituir la URL predeterminada, abra el Editor del Registro, cree la clave del Registro: **HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows Server\OnlineBackup\LinkUrl**y, a continuación, asigne la URL personalizada como valor de la clave.  
  
-   Puede ocultar la tarea: Para ocultar la tarea, abra el Editor del Registro y cree la clave del Registro: **HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows Server\OnlineBackup\OnlineBackupInstalled**.  
  
## <a name="see-also"></a>Consulta también  
 [Crear y personalizar la imagen](Creating-and-Customizing-the-Image.md)   
 [Personalizaciones adicionales](Additional-Customizations.md)   
 [Preparación de la imagen para la implementación](Preparing-the-Image-for-Deployment.md)   
 [Probar la experiencia del cliente](Testing-the-Customer-Experience.md)