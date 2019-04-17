---
title: "Personalizar el inicio de sesión para la tarea de servicio de copia de seguridad en línea de Microsoft"
description: "Describe cómo usar Windows Server Essentials"
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
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/03/2017
---
# <a name="customize-sign-up-for-microsoft-online-backup-service-task"></a>Personalizar el inicio de sesión para la tarea de servicio de copia de seguridad en línea de Microsoft

>Se aplica a: Windows Server 2016 Essentials, Windows Server 2012 R2 Essentials, Windows Server 2012 Essentials

De manera predeterminada, la **registrarse para el servicio de copia de seguridad en línea de Microsoft** de tareas en la **dispositivos** ficha del panel abre el sitio Web de Microsoft Online Service de copia de seguridad. El sitio Web proporciona información sobre el servicio y ayuda a suscribirse al servicio y descargar el software necesario.  
  
 Puedes personalizar la **registrarse para el servicio de copia de seguridad en línea de Microsoft** tarea de dos maneras:  
  
-   Puedes reemplazar la dirección URL del sitio Web predeterminado con una dirección URL que representa una experiencia de usuario personalizada. Para reemplazar la dirección URL predeterminada, abre el Editor del registro, crea la clave del registro: **HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows Server\OnlineBackup\LinkUrl**y, a continuación, asigna la dirección URL personalizada como valor de la clave.  
  
-   Puedes ocultar la tarea. Para ocultar la tarea, abre el Editor del registro y crear la clave del registro: **HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows Server\OnlineBackup\OnlineBackupInstalled **.  
  
## <a name="see-also"></a>Consulta también  
 [Crear y personalizar la imagen](Creating-and-Customizing-the-Image.md)   
 [Personalizaciones adicionales](Additional-Customizations.md)   
 [Preparación de la imagen para la implementación](Preparing-the-Image-for-Deployment.md)   
 [Prueba la experiencia del cliente](Testing-the-Customer-Experience.md)