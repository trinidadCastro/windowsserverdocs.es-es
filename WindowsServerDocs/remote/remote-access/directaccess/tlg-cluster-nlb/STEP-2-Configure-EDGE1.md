---
title: Paso 2 configurar EDGE1
description: Obtenga información sobre cómo configurar DirectAccess en el servidor de EDGE1.
manager: brianlic
ms.topic: article
ms.assetid: 84457351-1ca7-4e7c-8e2c-53d55b1fcdc0
ms.author: lizross
author: eross-msft
ms.date: 08/07/2020
ms.openlocfilehash: 52f94c6625286e9243419a668381995a18b28fae
ms.sourcegitcommit: f8da45df984f0400922a8306855b0adfdaec71af
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/08/2021
ms.locfileid: "98039335"
---
# <a name="step-2-configure-edge1"></a>Paso 2 configurar EDGE1

>Se aplica a: Windows Server (canal semianual), Windows Server 2016

El siguiente procedimiento se realiza en el servidor de DirectAccess:

## <a name="to-configure-directaccess-on-edge1"></a>Para configurar DirectAccess en EDGE1

1.  En la pantalla **Inicio** , escriba **RAMgmtUI.exe** y, a continuación, presione Entrar. Si aparece el cuadro de diálogo **Control de cuentas de usuario**, confirme que la acción que se muestra es la esperada y, a continuación, haga clic en **Sí**.

2.  En el panel izquierdo de la consola de administración de acceso remoto, haga clic en **configuración**.

3.  En el panel central de la consola, en el área **paso 2 servidor de acceso remoto** , haga clic en **Editar**.

4.  En el Asistente para la **instalación del servidor de acceso remoto** , haga clic en **configuración de prefijo**. En la **página Configuración de prefijo** , en **prefijo IPv6 asignado a equipos cliente de DirectAccess**, escriba **2001: db8:1: 1000::/59** y, a continuación, haga clic en **siguiente**.

5.  Haga clic en **Finalizar**

6.  En el panel central de la consola, haga clic en **Finalizar**.

7.  En el cuadro de diálogo **revisión de acceso remoto** , revise las opciones de configuración y, a continuación, haga clic en **aplicar**. En el cuadro de diálogo **Aplicar la configuración del Asistente para instalación de acceso remoto**, haga clic en **Cerrar**.
