---
title: Conversión de un disco MBR (registro de arranque maestro) en un disco GPT (tabla de particiones GUID)
description: Se describe cómo convertir un disco de registro de arranque maestro (MBR) en un disco de tabla de particiones GUID (GPT)
ms.date: 06/07/2019
ms.prod: windows-server
ms.technology: storage
ms.topic: article
author: JasonGerend
manager: brianlic
ms.author: jgerend
ms.openlocfilehash: 6bd97802fbef342520e92a857a1a53acf3e8d7a3
ms.sourcegitcommit: 3a3d62f938322849f81ee9ec01186b3e7ab90fe0
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/23/2020
ms.locfileid: "71385941"
---
# <a name="convert-an-mbr-disk-into-a-gpt-disk"></a>Conversión de un disco MBR en un disco GPT

> **Se aplica a:** Windows 10, Windows 8.1, Windows Server (canal semianual), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2 y Windows Server 2012

Los discos MBR (registro de arranque maestro) usan la tabla de particiones de BIOS estándar. Los discos GPT (tabla de particiones GUID) usan Unified Extensible Firmware Interface (UEFI). Una ventaja de los discos GPT es que puedes tener más de cuatro particiones en cada disco. GPT también se requiere para discos de más de dos terabytes (TB).

Puedes cambiar un disco con un estilo de partición MBR al estilo de partición GPT siempre que no incluya particiones o volúmenes.

> [!NOTE]
> Antes de convertir un disco, realiza una copia de seguridad de sus datos y cierra todos los programas que accedan al mismo.

> [!NOTE]
> Debes ser miembro del grupo **Operadores de copia de seguridad** o **Administradores**, como mínimo, para completar estos pasos.

## <a name="converting-using-the-windows-interface"></a>Conversión mediante la interfaz de Windows

1.  Realiza una copia de seguridad o mueve los datos del disco MBR básico que quieras convertir en un disco GPT.

2.  Si el disco incluye particiones o volúmenes, haz clic con el botón derecho en cada uno de ellos y luego haz clic en **Eliminar partición** o **Eliminar volumen**.

3.  Haz clic con el botón derecho en el disco MBR que quieres convertir en un disco GPT y luego haz clic en **Convertir en disco GPT**.

## <a name="converting-using-a-command-line"></a>Conversión desde la línea de comandos

Sigue estos pasos para convertir un disco MBR vacío en un disco GPT. También puedes usar una herramienta, MBR2GPT.EXE, pero el proceso es un poco complicado (para más información, consulta [Conversión de una partición MBR en GPT](https://docs.microsoft.com/windows/deployment/mbr-to-gpt)).

1.  Realiza una copia de seguridad o mueve los datos del disco MBR básico que quieras convertir en un disco GPT.

2.  Abre un símbolo del sistema con privilegios elevados, para lo que debes hacer clic con el botón derecho en **Símbolo del sistema** y luego Elegir **Ejecutar como administrador**.

3. Escriba `diskpart`. Si el disco no contiene particiones ni volúmenes, dirígete al paso 6.

4.  En el símbolo del sistema **DISKPART**, escribe `list disk`. Anota el número de disco que quieres convertir.

5.  En el símbolo del sistema **DISKPART**, escribe `select disk <disknumber>`.

6.  En el símbolo del sistema **DISKPART**, escribe `clean`.

    > [!NOTE]
    > Al ejecutar el comando **clean**, se eliminarán todas las particiones o volúmenes del disco.

7.  En el símbolo del sistema **DISKPART**, escribe `convert gpt`.

| Value  | Descripción  |
| ----- | ---- |
| **list disk** | Muestra una lista de discos e información sobre ellos, como su tamaño, la cantidad de espacio disponible, si se trata de un disco básico o dinámico y si el disco utiliza el estilo de partición de Registro de arranque maestro (MBR) o Tabla de particiones GUID (GPT). El disco marcado con un asterisco (*) tiene el foco. |
| **select disk** *disknumber* | Selecciona el disco especificado, donde *disknumber* es el número de disco y el que recibe el foco. |
| **clean** | Quita todas las particiones o volúmenes del disco con el foco.  |
| **convert gpt**| Convierte un disco básico vacío con el estilo de partición de registro de arranque maestro (MBR) en un disco básico con el estilo de partición de tabla de particiones GUID (GPT). |

## <a name="see-also"></a>Consulte también

-   [Notación de sintaxis de línea de comandos](https://technet.microsoft.com/library/cc742449(v=ws.11).aspx)