---
title: gpt
description: 'Tema de los comandos de Windows para ***- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 1d6f9029-807f-4420-a336-36669b5361bc
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 753ad622456280f22e8c4c209452cb17af75ce24
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59873476"
---
# <a name="gpt"></a>gpt

>Se aplica a: Windows Server (canal semianual), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

En discos (gpt) de la tabla de particiones GUID básicos, asigna los atributos gpt a la partición con el foco.  atributos de partición GPT ofrecen información adicional sobre el uso de la partición. Algunos atributos son específicos del tipo de particiones GUID.

> [!CAUTION]
> Cambiar los atributos gpt puede provocar que los volúmenes de datos básicos producir un error que se asigne letras de unidad o para evitar que el sistema de archivos de montaje. No debe cambiar los atributos gpt a menos que sea fabricante de equipos originales (OEM) o un profesional de TI que cuente con experiencia con los discos gpt.
## <a name="syntax"></a>Sintaxis
```
gpt attributes=<n>
```
## <a name="parameters"></a>Parámetros
|Parámetro|Descripción|
|-------|--------|
|attributes=<n>|Especifica el valor del atributo que desea aplicar a la partición con el foco. El campo de atributo gpt es un campo de 64 bits que contiene dos subcampos. El campo superior sólo se interpreta en el contexto de la Id. de partición, mientras que el campo inferior es común a todos los Id. de partición. Los valores aceptados son:<br /><br />-   **0x0000000000000001**. Especifica que la partición se requiere en el equipo para funcionar correctamente.<br />-   **0x8000000000000000**. Especifica que la partición no recibirán una letra de unidad de forma predeterminada cuando se mueve el disco a otro equipo o cuando el disco se ve por primera vez un equipo.<br />-   **0x4000000000000000**. Oculta el volumen de la partición. Es decir, la partición no se ha detectado por el Administrador de montaje.<br />-   **0x2000000000000000**. Especifica que la partición es una instantánea de otra partición.<br />-   **0x1000000000000000**. Especifica que la partición es de solo lectura. Este atributo impide que se escriben en el volumen.<br /><b />Para obtener más información acerca de estos atributos, consulte la sección de atributos al [create_PARTITION_PARAMETERS estructura](https://go.microsoft.com/fwlink/?LinkId=203812) (https://go.microsoft.com/fwlink/?LinkId=203812).|
## <a name="remarks"></a>Comentarios
-   La partición de sistema EFI contiene únicamente el código binario necesario para iniciar el sistema operativo. Esto facilita para los archivos binarios de OEM o archivos binarios específicos a un sistema operativo que se colocarán en otras particiones.
-   Debe seleccionarse una partición gpt básico para que esta operación se realice correctamente. Use la **seleccione partición** comando para seleccionar una partición gpt básico y desplace el foco a ella.
## <a name="BKMK_examples"></a>Ejemplos
Si mueve un disco gpt a un nuevo equipo y le gustaría evitar que ese equipo automáticamente asigna una letra de unidad a la partición con el foco, tipo:
```
gpt attributes=0x8000000000000000
```

