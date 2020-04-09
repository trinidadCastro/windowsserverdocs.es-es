---
title: volumen de atributos
description: El tema comandos de Windows para el **volumen de atributos**, que muestra, establece o borra los atributos de un volumen.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: e40e8284-3d57-4de8-a46c-e4ade34a0d53
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 00991cdba57f0728cfa348dea2b0916ad758b34a
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/08/2020
ms.locfileid: "80851238"
---
# <a name="attributes-volume"></a>volumen de atributos

>Se aplica a: Windows Server (canal semianual), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Muestra, establece o borra los atributos de un volumen.

## <a name="syntax"></a>Sintaxis  

```
attributes volume [{set | clear}] [{hidden | readonly | nodefaultdriveletter | shadowcopy}] [noerr]  
```  
  
### <a name="parameters"></a>Parámetros  
  
| Parámetro | Descripción |  
| ------- | -------- |  
| set | Establece el atributo especificado del volumen que tiene el foco. |  
| clear | Borra el atributo especificado del volumen que tiene el foco. |  
| readonly | Especifica que el volumen es de sólo lectura. |  
| hidden | Especifica que el volumen está oculto. |  
| nodefaultdriveletter | Especifica que el volumen no recibe una letra de unidad de forma predeterminada. |  
| shadowcopy | Especifica que el volumen es un volumen de instantánea. |  
| noerr | Sólo para scripting. Cuando se detecta un error, DiskPart sigue procesando los comandos como si no hubiera ningún error. Sin este parámetro, un error hace que DiskPart salga con un código de error. |  
  
## <a name="remarks"></a>Comentarios  
  
- En los discos básicos de registro de arranque maestro (MBR), los parámetros **Hidden**, **ReadOnly**y **nodefaultdriveletter** se aplican a todos los volúmenes del disco.  
  
- En los discos básicos de tabla de particiones GUID (GPT) y en discos MBR y GPT dinámicos, los parámetros **Hidden**, **ReadOnly**y **nodefaultdriveletter** solo se aplican al volumen seleccionado.  
  
- Se debe seleccionar un volumen para que el comando de **volumen de atributos** se ejecute correctamente. Use el comando **seleccionar volumen** para seleccionar un volumen y cambiar el foco a él.  
  
## <a name="examples"></a><a name=BKMK_examples></a>Example

Para mostrar los atributos actuales en el volumen seleccionado, escriba:  
  
```
attributes volume  
```  
  
Para establecer el volumen seleccionado como oculto y de solo lectura, escriba:  
  
```
attributes volume set hidden readonly  
```  
  
Para quitar los atributos oculto y de solo lectura del volumen seleccionado, escriba:  
  
```
attributes volume clear hidden readonly  
```  
  
## <a name="additional-references"></a>Referencias adicionales  

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)