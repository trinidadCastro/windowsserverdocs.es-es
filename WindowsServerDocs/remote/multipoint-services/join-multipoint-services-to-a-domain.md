---
title: Únase a MultiPoint Services a un dominio (opcional)
Description: Proporciona los pasos para unirse a MultiPoint Services a su dominio
ms.custom: na
ms.date: 07/22/2016
ms.prod: windows-server-threshold
ms.technology: multipoint-services
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 623b7c21-dcbb-402e-8b5a-8e434cd225bd
author: evaseydl
manager: scottman
ms.author: evas
ms.openlocfilehash: af5dd1f16e011161bbcf72c21c21088721ac1243
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59827046"
---
# <a name="join-the-multipoint-services-computer-to-a-domain-optional"></a>Unir el equipo de MultiPoint Services a un dominio (opcional)
Si tendrá acceso a su equipo de MultiPoint Services a través de un dominio de Active Directory, el siguiente paso es agregar el equipo al dominio.  
  
> [!IMPORTANT]  
> Antes de unir el equipo a un dominio, debe comprobar la zona horaria. Para obtener instrucciones, consulte [establecer la fecha, hora y zona horaria](Set-the-date--time--and-time-zone.md).  
   
1.  En la pantalla **Inicio**, abra el **Panel de control**. Haga clic en **sistema y seguridad**y, a continuación, haga clic en **sistema**.  
  
2.  En **Configuración de nombre, dominio y grupo de trabajo del equipo**, haga clic en **Cambiar configuración**.  
  
3.  En el **nombre_equipo** , haga clic **cambio**.  
  
4.  En el **cambios de dominio o el nombre de equipo** cuadro de diálogo, seleccione **dominio**, escriba el nombre del dominio y haga clic en **Aceptar**y, a continuación, siga los pasos del Asistente para completar la proceso.  
  
5.  Una vez reiniciado el equipo, inicie sesión como administrador y espere abrir el Administrador de MultiPoint.  
  
> [!IMPORTANT]  
> Para asegurarse de que la implementación de MultiPoint Services dominio funciona correctamente, deberá configurar un par de directivas de grupo y actualizar el registro. Para obtener información, consulte [configurar directivas de grupo para una implementación de dominio](https://technet.microsoft.com/library/dn265982.aspx).  