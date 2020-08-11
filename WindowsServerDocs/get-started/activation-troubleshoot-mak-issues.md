---
title: Problemas conocidos de activación de MAK
description: Describe los problemas comunes que pueden producirse durante el proceso de activación de MAK, y proporciona soluciones e instrucciones.
ms.topic: troubleshooting
ms.date: 10/3/2019
author: Teresa-Motiv
ms.author: v-tea
manager: dcscontentpm
ms.localizationpriority: medium
ms.openlocfilehash: ed9be4f46dccb525d4207378e1695d12a0c648ed
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/07/2020
ms.locfileid: "87972352"
---
# <a name="mak-activation-known-issues"></a>Problemas conocidos de activación de MAK

En este artículo se describen los problemas comunes que pueden producirse durante las activaciones de la Clave de activación múltiple (MAK), y se proporcionan instrucciones para solucionar esos problemas.

## <a name="how-can-i-tell-whether-my-computer-is-activated"></a>¿Cómo puedo saber si mi equipo está activado?

En el equipo, abre el panel de control del **Sistema** y busca **Windows está activado**. Como alternativa, ejecuta Slmgr.vbs y usa la opción de línea de comandos **/dli**.

## <a name="the-computer-does-not-activate-over-the-internet"></a>El equipo no se activa a través de Internet

Asegúrate de que los puertos necesarios estén abiertos en el firewall. Para obtener una lista de puertos, consulta la [Guía de implementación de la activación por volumen](https://go.microsoft.com/fwlink/?linkid=150083).

## <a name="internet-and-telephone-activation-fail"></a>La activación por Internet y por teléfono no funcionan

Ponte en contacto el Centro de activación de Microsoft local. Para ver los números de teléfono de los Centros de activación de Microsoft en todo el mundo, ve a [Números de teléfono internacionales del Centro de activación de licencias de Microsoft](https://www.microsoft.com/Licensing/existing-customer/activation-centers). Asegúrate de proporcionar la información del contrato de licencias por volumen y el comprobante de compra cuando llames.

## <a name="slmgrvbs-ato-returns-an-error-code"></a>Slmgr.vbs /ato devuelve un código de error

Si Slmgr.vbs devuelve un código de error hexadecimal, establece el mensaje de error correspondiente ejecutando el siguiente script:

```cmd
slui.exe 0x2a 0x <ErrorCode>
```

Para obtener más información sobre los códigos de error específicos y cómo solucionarlos, consulta [Resolución de problemas de códigos de error de activación](activation-error-codes.md).
