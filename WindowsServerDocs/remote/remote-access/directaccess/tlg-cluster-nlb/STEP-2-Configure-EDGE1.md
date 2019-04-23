---
title: PASO 2 Configurar EDGE1
description: 'En este tema forma parte de la Guía del laboratorio de pruebas: demostrar DirectAccess en un clúster con NLB de Windows para Windows Server 2016'
manager: brianlic
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology:
- networking-da
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 84457351-1ca7-4e7c-8e2c-53d55b1fcdc0
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 6c31e65fc7210849564bd0541085322a7a6c284e
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59838586"
---
# <a name="step-2-configure-edge1"></a>PASO 2 Configurar EDGE1

>Se aplica a: Windows Server (canal semianual), Windows Server 2016

El siguiente procedimiento se realiza en el servidor de DirectAccess:

## <a name="to-configure-directaccess-on-edge1"></a>Para configurar DirectAccess en EDGE1
  
1.  En el **iniciar** , escriba**RAMgmtUI.exe**, y, a continuación, presione ENTRAR. Si aparece el cuadro de diálogo **Control de cuentas de usuario** , confirme que la acción que se muestra es la esperada y, a continuación, haga clic en **Sí**.  
  
2.  En la consola de administración de acceso remoto, en el panel izquierdo, haga clic en **configuración**.  
  
3.  En el panel central de la consola, en el **paso 2: servidor de acceso remoto** área, haga clic en **editar**.  
  
4.  En el **instalación del servidor de acceso remoto** asistente, haga clic en **configuración de prefijo**. En el **configuración de prefijo** página **prefijo IPv6 asignado a los equipos cliente de DirectAccess**, escriba **2001:db8:1:1000:: / 59**y, a continuación, haga clic en **siguiente** .  
  
5.  Haga clic en **Finalizar**.  
  
6.  En el panel central de la consola, haga clic en **finalizar**.  
  
7.  En el **revisión de acceso remoto** cuadro de diálogo, revisar la configuración predeterminada y, a continuación, haga clic en **aplicar**. En el cuadro de diálogo **Aplicar la configuración del Asistente para instalación de acceso remoto**, haga clic en **Cerrar**.
