---
title: Actualizar la directiva de grupo
description: Este tema forma parte de la Guía de implementación de servidores de certificados para las implementaciones inalámbricas y cableadas 802.1X
manager: brianlic
ms.topic: article
ms.assetid: 65b36794-bb09-4c1b-a2e7-8fc780893d97
ms.prod: windows-server-threshold
ms.technology: networking
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 83dd48297535aafe30e48fe37010d81b279f4c91
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59863456"
---
# <a name="refresh-group-policy"></a>Actualizar la directiva de grupo

>Se aplica a: Windows Server (canal semianual), Windows Server 2016

Se puede usar este procedimiento para actualizar manualmente la directiva de grupo en el equipo local. Si la inscripción automática de certificados está configurada y funciona correctamente, al actualizar la directiva de grupo, la entidad de certificación (CA) inscribe un certificado para el equipo local de forma automática.  
  
> [!NOTE]  
> La directiva de grupo se actualiza automáticamente cuando reinicia el equipo del miembro de dominio o cuando un usuario inicia sesión en el equipo del miembro de dominio. Además, la directiva de grupo se actualiza periódicamente. De manera predeterminada, esta actualización periódica se realiza cada 90 minutos con una diferencia aleatoria de hasta 30 minutos.  
  
Pertenencia a **administradores**, o equivalente, es lo mínimo necesario para completar este procedimiento.  
  
### <a name="to-refresh-group-policy-on-the-local-computer"></a>Para actualizar la directiva de grupo en el equipo local  
  
1.  En el equipo donde se instala NPS, abra Windows PowerShell&reg; mediante el icono en la barra de tareas.  
  
2.  En el símbolo del sistema de Windows PowerShell, escriba **gpupdate**, y, a continuación, presione ENTRAR.  
  


