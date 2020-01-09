---
title: La conexión TCP se anula durante la negociación de la validación
description: Presenta la forma de solucionar el problema de SMB cuando se anula la conexión TCP durante la validación Negotiate.
author: Deland-Han
manager: dcscontentpm
audience: ITPro
ms.topic: article
ms.author: delhan
ms.date: 12/25/2019
ms.openlocfilehash: 3455b4ac0a2706f80702378dda02c1877af219ca
ms.sourcegitcommit: 8cf04db0bc44fd98f4321dca334e38c6573fae6c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/03/2020
ms.locfileid: "75654626"
---
# <a name="tcp-connection-is-aborted-during-validate-negotiate"></a>La conexión TCP se anula durante la negociación de la validación

En el seguimiento de red del problema SMB, observará que se ha anulado un restablecimiento de TCP durante el proceso de validación de negociación. En este artículo se describe cómo solucionar problemas de la situación.

## <a name="cause"></a>Causa

Este problema puede deberse a un error de validación de negociación. Esto suele ocurrir porque un acelerador de WAN modifica el paquete de negociación SMB original.

Microsoft ya no permite modificar el paquete de negociación de validación por cualquier motivo. Esto se debe a que este comportamiento crea un riesgo de seguridad grave.

Los siguientes requisitos se aplican al paquete de validación de negociación:

- El proceso Validate Negotiate usa el comando FSCTL\_VALIDATE\_NEGOTIATE\_INFO.

- La respuesta de validación de negociación debe estar firmada. De lo contrario, se anulará la conexión.

- Debe comparar el FSCTL\_validar\_negociar los mensajes de información de\_con los mensajes de negociación para asegurarse de que no se ha cambiado nada.

## <a name="workaround"></a>Solución alternativa

Puede deshabilitar temporalmente el proceso de validación de negociación. Para ello, busque la siguiente subclave del registro:

**HKEY\_\_de equipo LOCAL\\SYSTEM\\CurrentControlSet\\Services\\LanmanWorkstation\\parámetros**

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

[3.3.5.15.12 controlar una solicitud Validate Negotiate info](https://docs.microsoft.com/openspecs/windows_protocols/ms-smb2/0b7803eb-d561-48a4-8654-327803f59ec6)

[3.2.5.14.12 controlar una respuesta de validación de información de negociación](https://docs.microsoft.com/openspecs/windows_protocols/ms-smb2/6a5bc90d-3c08-4498-905b-e7dab30b2e0e)
