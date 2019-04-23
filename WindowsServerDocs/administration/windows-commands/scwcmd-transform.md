---
title: Transformación scwcmd
description: 'Tema de los comandos de Windows para ***- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 640dd892-0bb9-416d-8318-60a26605bcf4
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 8a6a6e37c2c2a362f3aa0aeadef615ff5065713f
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59843816"
---
# <a name="scwcmd-transform"></a>Scwcmd: transform

> Se aplica a: Windows Server 2012 R2, Windows Server 2012

Transforma un archivo de directiva de seguridad generado mediante el Asistente para configuración de seguridad (SCW) en una nueva directiva objeto grupo (GPO) en Active Directory Domain Services. La operación de transformación no modifica cualquier configuración en el servidor donde se lleva a cabo. Una vez completada la operación de transformación, un administrador debe vincular el GPO a las unidades organizativas deseadas para implementar la directiva a los servidores.

Se necesitan credenciales de administrador para completar la operación de transformación.

> [!IMPORTANT]
> Configuración de directiva de seguridad de Internet Information Services (IIS) no se puede implementar mediante la directiva de grupo.</br>> Inicia las directivas de firewall que muestran aplicaciones aprobadas no deben implementarse en servidores, a menos que el servicio de Firewall de Windows inicia automáticamente cuando el servidor por última vez.

Para obtener ejemplos de cómo se puede usar este comando, consulte [ejemplos](#BKMK_Examples).

## <a name="syntax"></a>Sintaxis

```
scwcmd transform /p:<Policyfile.xml> /g:<GPODisplayName>
```

### <a name="parameters"></a>Parámetros

|Parámetro|Descripción|
|---------|-----------|
|/p:\<Policyfile.xml>|Especifica la ruta de acceso y el nombre del archivo de directiva .xml que se debe aplicar. Este parámetro debe especificarse.|
|/g:\<GPODisplayName>|Especifica el nombre para mostrar del GPO. Este parámetro debe especificarse.|
|/?|Muestra la ayuda en el símbolo del sistema.|

## <a name="remarks"></a>Comentarios

Scwcmd.exe solo está disponible en equipos que ejecutan Windows Server 2008 R2, Windows Server 2008 o Windows Server 2003.

## <a name="BKMK_Examples"></a>Ejemplos

Para crear un GPO llamado FileServerSecurity desde un archivo denominado FileServerPolicy.xml, escriba:
```
scwcmd transform /p:FileServerPolicy.xml /g:FileServerSecurity
```

#### <a name="additional-references"></a>Referencias adicionales

-   [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)