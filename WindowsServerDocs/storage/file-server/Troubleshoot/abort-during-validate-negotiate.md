---
title: La conexión TCP se anula durante la negociación de la validación
description: Presenta la forma de solucionar el problema de SMB cuando se anula la conexión TCP durante la validación Negotiate.
author: Deland-Han
manager: dcscontentpm
ms.topic: article
ms.author: delhan
ms.date: 12/25/2019
ms.openlocfilehash: 762ad3b20c389771a9c0e6783d2a6fb1e73b8f0d
ms.sourcegitcommit: d5e27c1f2f168a71ae272bebf8f50e1b3ccbcca3
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/23/2020
ms.locfileid: "86961007"
---
# <a name="tcp-connection-is-aborted-during-validate-negotiate"></a>La conexión TCP se anula durante la negociación de la validación

En el seguimiento de red del problema SMB, observará que se ha anulado un restablecimiento de TCP durante el proceso de validación de negociación. En este artículo se describe cómo solucionar problemas de la situación.

## <a name="cause"></a>Causa

Este problema puede deberse a un error de validación de negociación. Esto suele ocurrir porque un acelerador de WAN modifica el paquete de negociación SMB original.

Microsoft ya no permite modificar el paquete de negociación de validación por cualquier motivo. Esto se debe a que este comportamiento crea un riesgo de seguridad grave.

Los siguientes requisitos se aplican al paquete de validación de negociación:

- El proceso de validación de negociación usa \_ el \_ comando FSCTL Validate Negotiate \_ info.

- La respuesta de validación de negociación debe estar firmada. De lo contrario, se anulará la conexión.

- Debe comparar los mensajes de información de negociación de la validación de FSCTL \_ con los mensajes de \_ negociación para asegurarse de \_ que no se ha cambiado nada.

## <a name="workaround"></a>Solución alternativa

Puede deshabilitar temporalmente el proceso de validación de negociación. Para ello, busque la siguiente subclave del registro:

**HKEY \_ local \_ Machine \\ System \\ CurrentControlSet \\ Services \\ LanmanWorkstation \\ Parameters**

En la clave **Parameters** , establezca **RequireSecureNegotiate** en **0**.

En Windows PowerShell, puede ejecutar el siguiente comando para establecer este valor:

```PowerShell
Set-ItemProperty -Path "HKLM:\SYSTEM\CurrentControlSet\Services\LanmanWorkstation\Parameters" RequireSecureNegotiate -Value 0 -Force
```

> [!NOTE]
> No se puede deshabilitar el proceso de validación de negociación en Windows 10, Windows Server 2016 o versiones posteriores de Windows.

Si el cliente o el servidor no admiten el comando validar negociación, puede solucionar este problema estableciendo la firma SMB en requerido. La firma SMB se considera más segura que validar Negotiate. Sin embargo, también puede haber degradación del rendimiento si se requiere la firma.

## <a name="reference"></a>Referencia

Para obtener más información, consulta los artículos siguientes:

[3.3.5.15.12 controlar una solicitud Validate Negotiate info](/openspecs/windows_protocols/ms-smb2/0b7803eb-d561-48a4-8654-327803f59ec6)

[3.2.5.14.12 controlar una respuesta de validación de información de negociación](/openspecs/windows_protocols/ms-smb2/6a5bc90d-3c08-4498-905b-e7dab30b2e0e)
