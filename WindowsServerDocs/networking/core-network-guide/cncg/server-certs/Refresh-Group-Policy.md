---
title: Actualizar la directiva de grupo
description: Este tema forma parte de la guía de implementación de certificados de servidor para las implementaciones cableadas e inalámbricas de 802.1 X
manager: brianlic
ms.topic: article
ms.assetid: 65b36794-bb09-4c1b-a2e7-8fc780893d97
ms.prod: windows-server
ms.technology: networking
ms.author: lizross
author: eross-msft
ms.openlocfilehash: b9522909960470d9f5f3e183afbd97ab1b919019
ms.sourcegitcommit: da7b9bce1eba369bcd156639276f6899714e279f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/26/2020
ms.locfileid: "80318256"
---
# <a name="refresh-group-policy"></a>Actualizar la directiva de grupo

>Se aplica a: Windows Server (canal semianual), Windows Server 2016

Se puede usar este procedimiento para actualizar manualmente la directiva de grupo en el equipo local. Si la inscripción automática de certificados está configurada y funciona correctamente, al actualizar la directiva de grupo, la entidad de certificación (CA) inscribe un certificado para el equipo local de forma automática.  
  
> [!NOTE]  
> La directiva de grupo se actualiza automáticamente cuando reinicia el equipo del miembro de dominio o cuando un usuario inicia sesión en el equipo del miembro de dominio. Además, la directiva de grupo se actualiza periódicamente. De manera predeterminada, esta actualización periódica se realiza cada 90 minutos con una diferencia aleatoria de hasta 30 minutos.  
  
La pertenencia al grupo **administradores**, o equivalente, es lo mínimo necesario para completar este procedimiento.  
  
### <a name="to-refresh-group-policy-on-the-local-computer"></a>Para actualizar la directiva de grupo en el equipo local  
  
1.  En el equipo donde está instalado NPS, abra el&reg; de Windows PowerShell mediante el icono de la barra de tareas.  
  
2.  En el símbolo del sistema de Windows PowerShell, escriba **gpupdate**y presione Entrar.  
  


