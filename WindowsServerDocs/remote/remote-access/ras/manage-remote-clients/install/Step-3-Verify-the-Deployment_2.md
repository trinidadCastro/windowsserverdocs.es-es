---
title: Paso 3 comprobación de la implementación
description: Este tema forma parte de la guía administrar los clientes de DirectAccess de forma remota en Windows Server 2016.
manager: brianlic
ms.topic: article
ms.assetid: 6a78a078-d2e7-4cbd-b8d5-20cfb6d1524b
ms.author: lizross
author: eross-msft
ms.date: 08/07/2020
ms.openlocfilehash: 9a3555364690228ec8723ee8c3123edaa4606c06
ms.sourcegitcommit: 40905b1f9d68f1b7d821e05cab2d35e9b425e38d
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/06/2021
ms.locfileid: "97947701"
---
# <a name="step-3-verify-the-deployment"></a>Paso 3 comprobación de la implementación

>Se aplica a: Windows Server (canal semianual), Windows Server 2016

En este tema se describe cómo comprobar que ha configurado correctamente la implementación para la administración remota de los clientes de DirectAccess.

### <a name="to-verify-proper-deployment"></a>Para comprobar la implementación correcta

1.  Conecte un equipo cliente de DirectAccess a la red corporativa y obtenga el objeto de directiva de grupo.

2.  En el equipo cliente, haga clic en el icono **conexiones de red** del área de notificación para tener acceso al administrador de medios de DirectAccess.

3.  Haga clic en **conexión de DirectAccess** y verá que el estado es **conectado localmente**.

4.  Quite el equipo de la red corporativa y conéctelo a una red pública.

5.  En un símbolo del sistema, escriba **NLTEST/dsgetdc: [nombre de dominio completo]**. Este comando comprobará que la red corporativa es accesible para el cliente. Si no se puede tener acceso al controlador de dominio, el siguiente mensaje de error mostrará informes de que el dominio no existe: ERROR_NO_SUCH_DOMAIN.



