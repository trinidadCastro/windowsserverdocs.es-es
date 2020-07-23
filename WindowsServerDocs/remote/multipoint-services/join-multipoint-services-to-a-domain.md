---
title: Unir Multipoint Services a un dominio (opcional)
Description: Proporciona los pasos para unir Multipoint Services a su dominio
ms.date: 07/22/2016
ms.prod: windows-server
ms.technology: multipoint-services
ms.topic: article
ms.assetid: 623b7c21-dcbb-402e-8b5a-8e434cd225bd
author: evaseydl
manager: scottman
ms.author: evas
ms.openlocfilehash: caa7611b47bd856648415358137609be41d4fca0
ms.sourcegitcommit: d5e27c1f2f168a71ae272bebf8f50e1b3ccbcca3
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/23/2020
ms.locfileid: "86959117"
---
# <a name="join-the-multipoint-services-computer-to-a-domain-optional"></a>Unir el equipo de Multipoint Services a un dominio (opcional)
Si va a tener acceso al equipo de Multipoint Services a través de un dominio de Active Directory, el paso siguiente consiste en agregar el equipo al dominio.  
  
> [!IMPORTANT]  
> Debe comprobar la zona horaria antes de unir el equipo a un dominio. Para obtener instrucciones, vea [establecer la fecha, la hora y la zona horaria](Set-the-date--time--and-time-zone.md).  
   
1.  En la pantalla **Inicio**, abra el **Panel de control**. Haga clic en **sistema y seguridad**y, a continuación, en **sistema**.  
  
2.  En **Configuración de nombre, dominio y grupo de trabajo del equipo**, haga clic en **Cambiar la configuración**.  
  
3.  En la pestaña **nombre de equipo** , haga clic en **cambiar**.  
  
4.  En el cuadro de diálogo cambios en el **dominio o el nombre del equipo** , seleccione **dominio**, escriba el nombre del dominio, haga clic en **Aceptar**y, a continuación, siga los pasos del Asistente para completar el proceso.  
  
5.  Una vez reiniciado el equipo, inicie sesión como administrador y espere a que se abra Multipoint Manager.  
  
> [!IMPORTANT]  
> Para asegurarse de que la implementación de dominio de Multipoint Services funciona correctamente, deberá configurar un par de directivas de grupo y actualizar el registro. Para obtener más información, consulte [configurar directivas de grupo para una implementación de dominio](/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/dn265982(v=ws.11)).  
