---
title: gpt
description: Artículo de referencia para el comando GPT, que asigna los atributos GPT a la partición que tiene el foco.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 1d6f9029-807f-4420-a336-36669b5361bc
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 40d3536f5a6be0bf520095e3ba61f75b7a2addc7
ms.sourcegitcommit: 2afed2461574a3f53f84fc9ec28d86df3b335685
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/02/2020
ms.locfileid: "85924612"
---
# <a name="gpt"></a>gpt

> Se aplica a: Windows Server (canal semianual), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2 y Windows Server 2012

En los discos básicos de tabla de particiones GUID (GPT), este comando asigna los atributos GPT a la partición que tiene el foco. Los atributos de partición GPT proporcionan información adicional sobre el uso de la partición. Algunos atributos son específicos del GUID de tipo de partición.

Debe elegir una partición GPT básica para que esta operación se realice correctamente. Use el [comando seleccionar partición](select-partition.md) para seleccionar una partición GPT básica y desplazar el foco a ella.

> [!CAUTION]
> El cambio de los atributos de GPT podría hacer que los volúmenes de datos básicos no se asignen a Letras de unidad o impedir el montaje del sistema de archivos. Le recomendamos encarecidamente que no cambie los atributos de GPT a menos que sea un fabricante de equipos originales (OEM) o un profesional de TI con experiencia en discos GPT.

## <a name="syntax"></a>Sintaxis

```
gpt attributes=<n>
```

### <a name="parameters"></a>Parámetros

| Parámetro | Descripción |
| --------- | ----------- |
| atributos =`<n>` | Especifica el valor del atributo que se va a aplicar a la partición que tiene el foco. El campo de atributo GPT es un campo de 64 bits que contiene dos subcampos. El campo superior sólo se interpreta en el contexto del Id. de partición, mientras que el campo inferior es común a todos los Id. de partición. Los valores aceptados son:<ul><li>**0x0000000000000001** : especifica que el equipo necesita la partición para funcionar correctamente.</li><li>**0x8000000000000000** : especifica que la partición no recibirá una letra de unidad de forma predeterminada cuando se mueva el disco a otro equipo, o cuando el disco se vea por primera vez en un equipo.</li><li>**0x4000000000000000** : oculta el volumen de una partición para que no lo detecte el administrador de montaje.</li><li>**0x2000000000000000** : especifica que la partición es una instantánea de otra partición.</li><li>**0x1000000000000000** : especifica que la partición es de solo lectura. Este atributo evita que se escriba en el volumen.</li></ul><p>Para obtener más información sobre estos atributos, vea la sección atributos en [Create_PARTITION_PARAMETERS estructura](https://docs.microsoft.com/windows/win32/api/vds/ns-vds-create_partition_parameters). |

#### <a name="remarks"></a>Comentarios

- La partición del sistema EFI solo contiene los archivos binarios necesarios para iniciar el sistema operativo. Esto facilita la colocación de archivos binarios o binarios OEM específicos de un sistema operativo en otras particiones.

### <a name="examples"></a>Ejemplos

Para evitar que el equipo asigne automáticamente una letra de unidad a la partición que tiene el foco, mientras mueve un disco GPT, escriba:

```
gpt attributes=0x8000000000000000
```

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)

- [seleccionar partición (comando)](select-partition.md)

- [Estructura de create_PARTITION_PARAMETERS](https://docs.microsoft.com/windows/win32/api/vds/ns-vds-create_partition_parameters)
