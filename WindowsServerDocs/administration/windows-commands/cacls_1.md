---
title: cacls
description: 'Tema de los comandos de Windows para ***- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: b5bdbaaa-4557-48b8-80df-e75ee0d2f27d
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 42f620a417f9d7bd06f779802e684e0196efc6a7
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/31/2019
ms.locfileid: "66434594"
---
# <a name="cacls"></a>cacls

>Se aplica a: Windows Server (canal semianual), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Muestra o modifica las listas de control de acceso discrecional (DACL) en los archivos especificados.  
## <a name="syntax"></a>Sintaxis  
```  
cacls <filename> [/t] [/m] [/l] [/s[:sddl]] [/e] [/c] [/g user:<perm>] [/r user [...]] [/p user:<perm> [...]] [/d user [...]]  
```  
### <a name="parameters"></a>Parámetros  

|        Parámetro        |                                                                                            Descripción                                                                                             |
|-------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|      \<filename\>       |                                                                            Obligatorio. Muestra las ACL de los archivos especificados.                                                                             |
|           /t            |                                                          cambia las ACL de los archivos especificados en el directorio actual y todos los subdirectorios.                                                          |
|           /m            |                                                                          los cambios de ACL de volúmenes montan en un directorio.                                                                           |
|           /l            |                                                                        Trabajar en el vínculo simbólico en Sí en comparación con el destino.                                                                         |
|         /s:sddl         |                                       reemplaza las ACL con las especificadas en la cadena de SDDL (no es válido con **/e**, **/g**, **/r**, **/p**, o **/d**).                                        |
|           /e            |                                                                                 editar la ACL en lugar de reemplazarlo.                                                                                  |
|           /c            |                                                                                 Continuar después de errores de acceso denegado.                                                                                  |
|    usuario/g:\<perm\>     |   GRANT especifica derechos de acceso de usuario.<br /><br />Valores válidos para el permiso:<br /><br />-n - ninguno<br />-r - lectura<br />-w - escritura<br />-c - cambio (escritura)<br />control total - f-   |
|      /r user [...]      |                                                                  Revocar los derechos de acceso del usuario especificado (solo es válido con **/e**).                                                                   |
| [/p user:\<perm\> [...] | reemplaza los derechos de acceso del usuario especificado.<br /><br />Valores válidos para el permiso:<br /><br />-n - ninguno<br />-r - lectura<br />-w - escritura<br />-c - cambio (escritura)<br />control total - f- |
|     [/d user [...]      |                                                                                    Denegar el acceso de usuario especificado.                                                                                     |
|           /?            |                                                                                Muestra la ayuda en el símbolo del sistema.                                                                                |

## <a name="remarks"></a>Comentarios  
- Este comando está desusado. Use [icacls](icacls.md) en su lugar.  
- Use la siguiente tabla para interpretar los resultados:  


  |      Salida       |                Entrada de control de acceso (ACE) se aplica a                |
  |-------------------|---------------------------------------------------------------------|
  |        OI         |               Objeto hereda. Esta carpeta y archivos.                |
  |        CI         |           Herencia de contenedor. Esta carpeta y subcarpetas.            |
  |        E/S         | Sólo heredar. La ACE no se aplica en el archivo o directorio actual. |
  | Ningún mensaje de salida |                          Sólo esta carpeta.                          |
  |     (OI) (CI)      |                 Esta carpeta, subcarpetas y archivos.                 |
  |   (OI) (CI) (IO)    |                     Sólo subcarpetas y archivos.                      |
  |     (CI) (IO)      |                          Sólo subcarpetas.                           |
  |     (OI) (IO)      |                             Sólo los archivos.                             |


- ¿Puede usar caracteres comodín ( **?** y **\\***) para especificar varios archivos.  
- Puede especificar más de un usuario.  

#### <a name="additional-references"></a>Referencias adicionales  
-   [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)   
-   [icacls](icacls.md)  
