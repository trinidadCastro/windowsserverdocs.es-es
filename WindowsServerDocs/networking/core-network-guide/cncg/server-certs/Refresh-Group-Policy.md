---
title: Actualizar la directiva de grupo
description: Este tema forma parte de la guía de implementación de certificados de servidor para las implementaciones cableadas e inalámbricas de 802.1 X
manager: brianlic
ms.topic: article
ms.assetid: 65b36794-bb09-4c1b-a2e7-8fc780893d97
ms.author: lizross
author: eross-msft
ms.date: 08/07/2020
ms.openlocfilehash: f379ce07682ceec4dcfff1395b316eee333a3ec5
ms.sourcegitcommit: 40905b1f9d68f1b7d821e05cab2d35e9b425e38d
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/06/2021
ms.locfileid: "97947821"
---
# <a name="refresh-group-policy"></a>Actualizar la directiva de grupo

>Se aplica a: Windows Server (canal semianual), Windows Server 2016

Se puede usar este procedimiento para actualizar manualmente la directiva de grupo en el equipo local. Si la inscripción automática de certificados está configurada y funciona correctamente, al actualizar la directiva de grupo, la entidad de certificación (CA) inscribe un certificado para el equipo local de forma automática.

> [!NOTE]
> La directiva de grupo se actualiza automáticamente cuando reinicia el equipo del miembro de dominio o cuando un usuario inicia sesión en el equipo del miembro de dominio. Además, la directiva de grupo se actualiza periódicamente. De manera predeterminada, esta actualización periódica se realiza cada 90 minutos con una diferencia aleatoria de hasta 30 minutos.

La pertenencia al grupo **administradores**, o equivalente, es lo mínimo necesario para completar este procedimiento.

### <a name="to-refresh-group-policy-on-the-local-computer"></a>Para actualizar la directiva de grupo en el equipo local

1.  En el equipo donde está instalado NPS, abra Windows PowerShell mediante &reg; el icono de la barra de tareas.

2.  En el símbolo del sistema de Windows PowerShell, escriba **gpupdate** y presione Entrar.



