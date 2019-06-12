---
title: Convertir un registro de arranque maestro (MBR) en un disco de tabla de particiones GUID (GPT)
description: Describe cómo convertir un registro de arranque maestro (MBR) en un disco de tabla de particiones GUID (GPT)
ms.date: 06/07/2019
ms.prod: windows-server-threshold
ms.technology: storage
ms.topic: article
author: JasonGerend
manager: brianlic
ms.author: jgerend
ms.openlocfilehash: 902a845bbe6a7e2a4d811aac0ea2990fb3557832
ms.sourcegitcommit: 6ef4986391607bb28593852d06cc6645e548a4b3
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 06/07/2019
ms.locfileid: "66812453"
---
# <a name="convert-an-mbr-disk-into-a-gpt-disk"></a>Convertir un disco MBR en un disco GPT

> **Se aplica a:** Windows 10, Windows 8.1, Windows Server (canal semianual), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Los discos de registro de arranque maestro (MBR) usan la tabla de particiones BIOS estándar. Los discos de tabla de particiones GUID (GPT) usan Unified Extensible Firmware Interface (UEFI). Una ventaja de los discos GPT es que puedes tener más de cuatro particiones en cada disco. GPT también se requiere para discos de más de dos terabytes (TB).

Puedes cambiar un disco de MBR al estilo de partición GPT siempre que el disco no incluya particiones o volúmenes.

> [!NOTE]
> Antes de convertir un disco, todos los datos de copia de seguridad y cierre todos los programas que se accede al disco.

> [!NOTE]
> Debes ser miembro del grupo **Operadores de copia de seguridad** o **Administradores**, como mínimo, para completar estos pasos.

## <a name="converting-using-the-windows-interface"></a>Conversión mediante la interfaz de Windows

1.  Realiza una copia de seguridad o mueve los datos del disco MBR básico que quieras convertir en un disco GPT.

2.  Si el disco incluye particiones o volúmenes, haz clic con el botón derecho en cada uno de ellos y luego haz clic en **Eliminar partición** o **Eliminar volumen**.

3.  Haz clic con el botón derecho en el disco MBR que quieres convertir en un disco GPT y luego haz clic en **Convertir en disco GPT**.

## <a name="converting-using-a-command-line"></a>Convertir la línea de comandos

Siga estos pasos para convertir un disco MBR vacío en un disco GPT. También hay un MBR2GPT. Vea herramienta EXE que puede usar, pero es un poco complicado - [partición convertir MBR a GPT](https://docs.microsoft.com/windows/deployment/mbr-to-gpt) para obtener más detalles.

1.  Realiza una copia de seguridad o mueve los datos del disco MBR básico que quieras convertir en un disco GPT.

2.  Abre un símbolo del sistema con privilegios elevados haciendo clic con el botón derecho en **Símbolo del sistema** y luego elige **Ejecutar como administrador**.

3. Escriba `diskpart`. Si el disco no incluye particiones ni volúmenes, continúa al paso 6.

4.  En el símbolo del sistema **DISKPART**, escribe `list disk`. Anota el número de disco que quieras convertir.

5.  En el símbolo del sistema **DISKPART**, escribe `select disk <disknumber>`.

6.  En el símbolo del sistema **DISKPART**, escribe `clean`.

    > [!NOTE]
    > Ejecutar el comando **clean** eliminará todas las particiones o volúmenes en el disco.

7.  En el símbolo del sistema **DISKPART**, escribe `convert gpt`.

| Valor  | Descripción  |
| ----- | ---- |
| **disco de la lista** | Muestra una lista de discos e información sobre ellos, como su tamaño, la cantidad de espacio libre disponible, si se trata de un disco básico o dinámico y si el disco utiliza el estilo de partición de Registro de arranque maestro (MBR) o Tabla de particiones GUID (GPT). El disco marcado con un asterisco (*) tiene el foco. |
| **Seleccione el disco** *númeroDeDisco* | Selecciona el disco especificado, donde *disknumber* es el número de disco y el que recibe el foco. |
| **clean** | Quita todas las particiones o volúmenes del disco con el foco.  |
| **convert gpt**| Convierte un disco básico vacío con el estilo de partición de registro de arranque maestro (MBR) en un disco básico con el estilo de partición de tabla de particiones GUID (GPT). |

## <a name="see-also"></a>Vea también

-   [Notación de sintaxis de línea de comandos](https://technet.microsoft.com/library/cc742449(v=ws.11).aspx)