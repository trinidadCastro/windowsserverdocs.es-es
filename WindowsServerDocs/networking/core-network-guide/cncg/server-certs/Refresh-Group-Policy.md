---
title: Directiva de grupo de actualización
description: Este tema es parte de la Guía de certificados de servidor de implementación para implementaciones de conexión inalámbrica y cableadas 802.1X
manager: brianlic
ms.topic: article
ms.assetid: 65b36794-bb09-4c1b-a2e7-8fc780893d97
ms.prod: windows-server-threshold
ms.technology: networking
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 4d9f5d38199f8cf3c0ffe46df4cd975cd9c56ff6
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2018
---
# <a name="refresh-group-policy"></a>Directiva de grupo de actualización

>Se aplica a: Windows Server (punto y anual canal), Windows Server 2016

Puedes usar este procedimiento para actualizar manualmente la directiva de grupo en el equipo local. Cuando se actualiza la directiva de grupo, si se configura la inscripción y funciona correctamente, el equipo local es inscribe automáticamente un certificado por la entidad de certificación (CA).  
  
> [!NOTE]  
> Directiva de grupo se actualiza automáticamente cuando se reinicie el equipo miembro del dominio o cuando un usuario inicia sesión en un equipo miembro del dominio. Además, directiva de grupo se actualiza periódicamente. De manera predeterminada, esta actualización periódica se realiza cada 90 minutos con un desplazamiento aleatorio de hasta 30 minutos.  
  
Pertenencia a **administradores**, o equivalente, es lo mínimo necesario para completar este procedimiento.  
  
### <a name="to-refresh-group-policy-on-the-local-computer"></a>Para actualizar la directiva de grupo en el equipo local  
  
1.  En el equipo donde está instalado NPS, abre Windows PowerShell&reg; mediante el icono en la barra de tareas.  
  
2.  En el símbolo del sistema de Windows PowerShell, escribe **gpupdate**, y, a continuación, presione ENTRAR.  
  


