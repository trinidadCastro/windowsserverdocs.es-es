---
title: cacls
description: Temas de comandos de Windows para cacls, que muestra o modifica las listas de control de acceso discrecional (DACL) en los archivos especificados.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: b5bdbaaa-4557-48b8-80df-e75ee0d2f27d
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 8cc6300563880a587b6544ec8cb61e9010ad6c2c
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/08/2020
ms.locfileid: "80848248"
---
# <a name="cacls"></a>cacls

>Se aplica a: Windows Server (canal semianual), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Muestra o modifica las listas de control de acceso discrecional (DACL) en los archivos especificados.  

## <a name="syntax"></a>Sintaxis  
```  
cacls <filename> [/t] [/m] [/l] [/s[:sddl]] [/e] [/c] [/g user:<perm>] [/r user [...]] [/p user:<perm> [...]] [/d user [...]]  
```  
#### <a name="parameters"></a>Parámetros  

|        Parámetro        |                                                                                            Descripción                                                                                             |
|-------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|      \<nombre de archivo\>       |                                                                            Obligatorio. Muestra las ACL de los archivos especificados.                                                                             |
|           /t            |                                                          cambia las ACL de los archivos especificados en el directorio actual y en todos los subdirectorios.                                                          |
|           /m            |                                                                          cambia las ACL de los volúmenes montados en un directorio.                                                                           |
|           /l            |                                                                        Trabaje en el propio vínculo simbólico frente al destino.                                                                         |
|         /s: SDDL         |                                       reemplaza las ACL por las especificadas en la cadena SDDL (no es válida con **/e**, **/g**, **/r**, **/p**o **/d**).                                        |
|           /e            |                                                                                 Edite ACL en lugar de reemplazarlo.                                                                                  |
|           /c            |                                                                                 Continuar después de errores de acceso denegado.                                                                                  |
|    /g usuario:\<Perm\>     |   Conceda derechos de acceso de usuario especificados.<p>Valores válidos para el permiso:<p>-n: ninguno<br />-r-lectura<br />-w-escritura<br />-c: cambiar (escribir)<br />-f: control total   |
|      /r usuario [...]      |                                                                  Revocar los derechos de acceso del usuario especificado (solo válido con **/e**).                                                                   |
| [/p usuario:\<Perm\> [...] | reemplazar los derechos de acceso del usuario especificado.<p>Valores válidos para el permiso:<p>-n: ninguno<br />-r-lectura<br />-w-escritura<br />-c: cambiar (escribir)<br />-f: control total |
|     [/d usuario [...]      |                                                                                    Denegar el acceso de usuario especificado.                                                                                     |
|           /?            |                                                                                Muestra la Ayuda en el símbolo del sistema.                                                                                |

## <a name="remarks"></a>Comentarios  
- Este comando está en desuso. En su lugar, use [icacls](icacls.md) .  
- Utilice la siguiente tabla para interpretar los resultados:  


  |      Salida       |                Entrada de control de acceso (ACE) se aplica a                |
  |-------------------|---------------------------------------------------------------------|
  |        OI         |               Herencia de objeto. Esta carpeta y archivos.                |
  |        CI         |           Herencia del contenedor. Esta carpeta y sus subcarpetas.            |
  |        E/S         | Heredar únicamente. La ACE no se aplica al archivo o directorio actual. |
  | Sin mensaje de salida |                          Solo esta carpeta.                          |
  |     OI IA      |                 Esta carpeta, subcarpetas y archivos.                 |
  |   OI IA IO    |                     Solo subcarpetas y archivos.                      |
  |     IA IO      |                          Solo subcarpetas.                           |
  |     OI IO      |                             Solo archivos.                             |


- Puede usar caracteres comodín ( **?** y **\\\*** ) para especificar varios archivos.  
- Puede especificar más de un usuario.  

## <a name="additional-references"></a>Referencias adicionales  
-   - [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)   
-   [icacls](icacls.md)  
