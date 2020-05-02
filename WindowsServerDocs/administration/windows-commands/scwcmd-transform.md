---
title: Scwcmd (transformación)
description: Tema de referencia de * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 640dd892-0bb9-416d-8318-60a26605bcf4
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 1f1116b42d356cc36f478089cdf487a38e792e87
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/01/2020
ms.locfileid: "82722119"
---
# <a name="scwcmd-transform"></a>Scwcmd: transform

> Se aplica a: Windows Server 2012 R2, Windows Server 2012

Transforma un archivo de directiva de seguridad generado mediante el Asistente para configuración de seguridad (SCW) en un nuevo objeto de directiva de grupo (GPO) en Active Directory Domain Services. La operación de transformación no cambia ninguna configuración en el servidor en el que se realiza. Una vez completada la operación de transformación, un administrador debe vincular el GPO a las unidades organizativas deseadas para implementar la Directiva en los servidores.

Se necesitan credenciales de administrador de dominio para completar la operación de transformación.

> [!IMPORTANT]
> La configuración de la Directiva de seguridad de Internet Information Services (IIS) no se puede implementar mediante directiva de grupo.</br>> las directivas de firewall que enumeran las aplicaciones aprobadas no se deben implementar en los servidores a menos que el servicio Firewall de Windows se inicie automáticamente cuando el servidor se inició por última vez.



## <a name="syntax"></a>Sintaxis

```
scwcmd transform /p:<Policyfile.xml> /g:<GPODisplayName>
```

#### <a name="parameters"></a>Parámetros

|Parámetro|Descripción|
|---------|-----------|
|/p:\<policyFile. XML>|Especifica la ruta de acceso y el nombre de archivo del archivo de directiva. XML que se debe aplicar. Se debe especificar este parámetro.|
|/g:\<GPODisplayName>|Especifica el nombre para mostrar del GPO. Se debe especificar este parámetro.|
|/?|Muestra la ayuda en el símbolo del sistema.|

## <a name="remarks"></a>Observaciones

Scwcmd. exe solo está disponible en equipos que ejecutan Windows Server 2008 R2, Windows Server 2008 o Windows Server 2003.

## <a name="examples"></a>Ejemplos

Para crear un GPO denominado FileServerSecurity desde un archivo denominado FileServerPolicy. XML, escriba:
```
scwcmd transform /p:FileServerPolicy.xml /g:FileServerSecurity
```

## <a name="additional-references"></a>Referencias adicionales

-   - [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)