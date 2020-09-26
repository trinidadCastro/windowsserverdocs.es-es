---
title: scwcmd transform
description: Artículo de referencia del comando scwcmd transform, que transforma un archivo de directiva de seguridad generado mediante el Asistente para configuración de seguridad (SCW) en un nuevo objeto de directiva de grupo (GPO) en Active Directory Domain Services.
ms.topic: reference
ms.assetid: 640dd892-0bb9-416d-8318-60a26605bcf4
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: bef3d9800d19ac0e3e574b4ef2e1195dcdc3baed
ms.sourcegitcommit: e164aeffc01069b8f1f3248bf106fcdb7f64f894
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/26/2020
ms.locfileid: "91389258"
---
# <a name="scwcmd-transform"></a>scwcmd transform

> Se aplica a: Windows Server 2012 R2 y Windows Server 2012

Transforma un archivo de directiva de seguridad generado mediante el Asistente para configuración de seguridad (SCW) en un nuevo objeto de directiva de grupo (GPO) en Active Directory Domain Services. La operación de transformación no cambia ninguna configuración en el servidor en el que se realiza. Una vez completada la operación de transformación, un administrador debe vincular el GPO a las unidades organizativas deseadas para implementar la Directiva en los servidores.

> [!IMPORTANT]
> Se necesitan credenciales de administrador de dominio para completar la operación de transformación.
>
> La configuración de la Directiva de seguridad de Internet Information Services (IIS) no se puede implementar mediante directiva de grupo.
>
> Las directivas de firewall que muestran aplicaciones aprobadas no se deben implementar en los servidores a menos que el servicio Firewall de Windows se inicie automáticamente cuando se inició el servidor por última vez.

## <a name="syntax"></a>Sintaxis

```
scwcmd transform /p:<policyfile.xml> /g:<GPOdisplayname>
```

### <a name="parameters"></a>Parámetros

| Parámetro | Descripción |
|--|--|
| /p`<policyfile.xml>` | Especifica la ruta de acceso y el nombre de archivo del archivo de directiva. XML que se debe aplicar. Se debe especificar este parámetro. |
| /g`<GPOdisplayname>` | Especifica el nombre para mostrar del GPO. Se debe especificar este parámetro. |
| /? | Muestra la ayuda en el símbolo del sistema. |

## <a name="examples"></a>Ejemplos

Para crear un GPO denominado *FileServerSecurity* desde un archivo denominado *FileServerPolicy.xml*, escriba:

```
scwcmd transform /p:FileServerPolicy.xml /g:FileServerSecurity
```

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)

- [comando scwcmd ANALYZE](scwcmd-analyze.md)

- [comando scwcmd configure](scwcmd-configure.md)

- [comando scwcmd Register](scwcmd-register.md)

- [comando de restauración de scwcmd](scwcmd-rollback.md)

- [comando scwcmd View](scwcmd-view.md)