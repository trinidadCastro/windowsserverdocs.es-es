---
title: Unir equipos a la nueva red1 de Windows Server Essentials
description: "Describe cómo usar Windows Server Essentials"
ms.custom: na
ms.date: 10/03/2016
ms.prod: windows-server-2016-essentials
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: d94de050-3300-4323-a5ea-c824cb9cecc9
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: c6abc11ba2ce8a9f1d32c6a884db6332586de78b
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/03/2017
---
# <a name="join-computers-to-the-new-windows-server-essentials-network1"></a>Unir equipos a la nueva red1 de Windows Server Essentials

>Se aplica a: Windows Server 2016 Essentials, Windows Server 2012 R2 Essentials, Windows Server 2012 Essentials

##  <a name="BKMK_JoinComputers"></a>   
 El siguiente paso del proceso de migración es unirse a los equipos cliente a la red de Windows Server Essentials de nuevo y actualizar la configuración de directiva de grupo.  
  
### <a name="domain-joined-client-computers"></a>Equipos cliente Unidos a un dominio  
 Busca **http://***destino nombreDeServidor***/ conecten** e instalar el software del conector de servidor de Windows como si se trata de un equipo nuevo. El proceso de instalación es el mismo para los equipos cliente Unidos a un dominio o no unidos a un dominio.  
  
> [!NOTE]
>  El software del conector de servidor de Windows no es compatible con equipos que ejecutan Windows XP o Windows Vista. Si tienes equipos que ejecutan Windows XP o Windows Vista que ya están unidos al dominio, puedes omitir este paso.  
  
### <a name="non-domain-joined-client-computers"></a>Equipos cliente no unidos a un dominio  
 Busca **http://***destino nombreDeServidor***/ conecten** e instalar el software del conector de servidor de Windows como si se trata de un equipo nuevo. El proceso de instalación es el mismo para los equipos cliente combinadas de no del dominio o unido a un dominio.  
  
> [!NOTE]
>  El software del conector de servidor de Windows no es compatible con equipos que ejecutan Windows XP o Windows Vista. Si tienes equipos que ejecutan Windows XP o Windows Vista que ya están unidos al dominio, puedes omitir este paso.  
  
### <a name="ensure-that-group-policy-has-updated"></a>Asegúrate de que se ha actualizado la directiva de grupo  
  
> [!NOTE]
>  Este es un paso opcional y sólo es necesario si el servidor de origen se configuró con una configuración personalizada de directiva de grupo, como la redirección de carpetas.  
  
 Mientras el servidor de origen y el servidor de destino aún están en línea, debes asegurarte de que la directiva de grupo se replica configuración desde el servidor de destino en los equipos cliente. Realiza los siguientes pasos en cada equipo cliente:  
  
1.  Abre una ventana de símbolo del sistema.  
  
2.  En el símbolo del sistema, escribe **GPRESULT /R**, y, a continuación, presione ENTRAR.  
  
3.  Revisa el resultado de la sección de directiva de grupo se aplicó desde: y asegurarse de el servidor de destino, enumera como **DestinationSrv.Domain.local**. Por ejemplo:  
  
    ```  
    USER SETTINGS  
    --------------  
        CN=User,OU=Users,DC=DOMAIN,DC=Local  
        Last time Group Policy was applied: 1/24/2011 at 1:26:27 PM  
        Group Policy was applied from:      DestinationSrv.Domain.local  
        Group Policy slow link threshold:   500 kbps  
        Domain Name:                        Domain  
        Domain Type:                        Windows 2008  
  
    ```  
  
4.  Si no aparece el servidor de destino, en un símbolo del sistema, escriba **comando gpupdate/force**, y, a continuación, presiona ENTRAR para actualizar la configuración de directiva de grupo. A continuación, volver a realizar el procedimiento anterior.  
  
5.  Si el servidor de destino no aparece, puede haber un error en la configuración de directiva de grupo o un error en la aplicación a este equipo cliente específico. Si el servidor de destino no aparece, realiza los siguientes pasos:  
  
    1.  Haz clic en **inicio**, haz clic en **ejecutar**, tipo **rsop.msc** (conjunto resultante de directivas), y, a continuación, presione ENTRAR.  
  
    2.  Expande el árbol con la X hasta que llegues a un nodo.  
  
    3.  Haz clic en el nodo y haz clic en **ver Error** para obtener información acerca de por qué se producen errores en la configuración de directiva de grupo en el equipo mostrado.
