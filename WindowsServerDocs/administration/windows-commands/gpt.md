---
title: GPT
description: Tema de referencia de * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 1d6f9029-807f-4420-a336-36669b5361bc
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 5eaa52329a08f85a2a97d89ff178039c5b883017
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/01/2020
ms.locfileid: "82724948"
---
# <a name="gpt"></a>GPT

> Se aplica a: Windows Server (canal semianual), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2 y Windows Server 2012

En los discos básicos de tabla de particiones GUID (GPT), asigna el atributo GPT a la partición que tiene el foco.  los atributos de partición GPT proporcionan información adicional sobre el uso de la partición. Algunos atributos son específicos del GUID de tipo de partición.

> [!CAUTION]
> El cambio de los atributos de GPT podría hacer que los volúmenes de datos básicos no se asignen a Letras de unidad o impedir el montaje del sistema de archivos. No debe cambiar los atributos GPT a menos que sea un fabricante de equipos originales (OEM) o un profesional de TI con experiencia en discos GPT.

## <a name="syntax"></a>Sintaxis

```
gpt attributes=<n>
```

### <a name="parameters"></a>Parámetros

|   Parámetro    |                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                               Descripción                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                |
|----------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| atributos =<n> | Especifica el valor del atributo que se va a aplicar a la partición que tiene el foco. El campo de atributo GPT es un campo de 64 bits que contiene dos subcampos. El campo superior sólo se interpreta en el contexto del Id. de partición, mientras que el campo inferior es común a todos los Id. de partición. Los valores aceptados son:<p>-   **0x0000000000000001**. Especifica que el equipo necesita la partición para funcionar correctamente.<br />-   **0x8000000000000000**. Especifica que la partición no recibirá una letra de unidad de forma predeterminada cuando se mueva el disco a otro equipo o cuando el disco se vea por primera vez en un equipo.<br />-   **0x4000000000000000**. Oculta el volumen de una partición. Es decir, el administrador de montaje no detectará la partición.<br />-   **0x2000000000000000**. Especifica que la partición es una instantánea de otra partición.<br />-   **0x1000000000000000**. Especifica que la partición es de solo lectura. Este atributo evita que se escriba en el volumen.<p>Para obtener más información sobre estos atributos, vea la sección atributos en [Create_PARTITION_PARAMETERS estructura](https://go.microsoft.com/fwlink/?LinkId=203812). |

## <a name="remarks"></a>Observaciones

- La partición del sistema EFI solo contiene los archivos binarios necesarios para iniciar el sistema operativo. Esto facilita la colocación de archivos binarios o binarios OEM específicos de un sistema operativo en otras particiones.
- Se debe seleccionar una partición GPT básica para que esta operación se realice correctamente. Use el comando **seleccionar partición** para seleccionar una partición GPT básica y desplazar el foco a ella.

## <a name="examples"></a>Ejemplos

  Si va a mover un disco GPT a un nuevo equipo y desea impedir que ese equipo asigne automáticamente una letra de unidad a la partición que tiene el foco, escriba:
  ```
  gpt attributes=0x8000000000000000
  ```
