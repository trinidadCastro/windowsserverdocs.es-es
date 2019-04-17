---
title: "Cambiar un disco dinámico por un disco básico"
description: "Describe cómo convertir un disco dinámico en un disco básico."
ms.date: 10/12/2017
ms.prod: windows-server-threshold
ms.technology: storage
ms.topic: article
author: JasonGerend
manager: brianlic
ms.author: jgerend
ms.openlocfilehash: 9675141ec02c2362702265a18f5405554dff6038
ms.sourcegitcommit: 583355400f6b0d880dc0ac6bc06f0efb50d674f7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/17/2017
---
# <a name="change-a-dynamic-disk-back-to-a-basic-disk"></a>Cambiar un disco dinámico por un disco básico

> **Se aplica a:** Windows 10, Windows 8.1, Windows Server (canal semianual), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

En este tema se describe cómo eliminar todo el contenido de un disco dinámico y luego convertirlo en un disco básico. Los discos dinámicos han dejado de usarse en Windows. Recomendamos usar la tecnología más reciente de [Espacios de almacenamiento](https://support.microsoft.com/help/12438/windows-10-storage-spaces) si quieres agrupar discos en volúmenes de mayor tamaño. Si quieres reflejar el volumen desde el que Windows se inicia, es posible que quieras usar un controlador RAID de hardware, como por ejemplo, el incluido en un gran número de placas base.

<br />

> [!WARNING]
> Para convertir un disco dinámico en un disco básico, debes eliminar todos los volúmenes del disco, borrando de forma permanente todos los datos del disco. Asegúrate de realizar copias de seguridad de todos los datos que quieras conservar antes de proceder.

## <a name="changing-a-dynamic-disk-back-to-a-basic-disk"></a>Cambiar un disco dinámico por un disco básico

-   [Usar la interfaz de Windows](#BKMK_WINUI)
-   [Usar una línea de comandos](#BKMK_CMD)

> [!NOTE]
> Debes ser miembro del grupo **Operadores de copia de seguridad** o **Administradores**, como mínimo, para completar estos pasos.

<a href="" id="BKMK_WINUI"></a>
#### <a name="to-change-a-dynamic-disk-back-to-a-basic-disk-using-the-windows-interface"></a>Para cambiar un disco dinámico por un disco básico mediante la interfaz de Windows
1.  Realiza una copia de seguridad de todos los volúmenes del disco que quieras convertir de dinámicos a básicos.

2.  En Administración de discos, haz clic con el botón derecho en cada volumen del disco dinámico que quieras convertir en un disco básico y luego haz clic en **Eliminar volumen** de cada volumen del disco.

3.  Cuando todos los volúmenes del disco se han eliminado, haz clic con el botón derecho en el disco y luego haz clic en **Convertir en disco básico**.


<a href="" id="BKMK_CMD"></a>
#### <a name="to-change-a-dynamic-disk-back-to-a-basic-disk-using-a-command-line"></a>Para cambiar un disco dinámico por un disco básico mediante una línea de comandos

1.  Realiza una copia de seguridad de todos los volúmenes del disco que quieras convertir de dinámicos a básicos.

2.  Abre un símbolo del sistema y escribe `diskpart`.

3.  En el símbolo del sistema **DISKPART**, escribe `list disk`. Anota el número de disco que quieras convertir a básico.

4.  En el símbolo del sistema **DISKPART**, escribe `select disk <disknumber>`.

5.  En el símbolo del sistema **DISKPART**, escribe `detail disk <disknumber>`.

6.  Para cada volumen del disco, en el símbolo de sistema **DISKPART**, escribe `select volume= <volumenumber>` y luego escribe `delete volume`.

7.  En el símbolo de sistema **DISKPART**, escribe `select disk <disknumber>` y especifica el número de disco del disco que quieres convertir en un disco básico.

8.  En el símbolo del sistema **DISKPART**, escribe `convert basic`.
 
<br /> <br />

| Valor  | Descripción |
| --- |---|
| <p>**list disk**</p>                         | <p>Muestra una lista de discos e información sobre ellos, como su tamaño, la cantidad de espacio libre disponible, si se trata de un disco básico o dinámico y si el disco utiliza el estilo de partición de Registro de arranque maestro (MBR) o Tabla de particiones GUID (GPT). El disco marcado con un asterisco (*) tiene el foco.</p> |
| <p>**select disk** <em>disknumber</em></p>   | <p>Selecciona el disco especificado, donde <em>disknumber</em> es el número de disco y el que recibe el foco.</p>  |
| <p>**detail disk** <em>disknumber</em></p>   | <p>Muestra las propiedades del disco seleccionado y los volúmenes de dicho disco.</p>  |
| <p>**select volume** <em>disknumber</em></p> | <p>Selecciona el volumen especificado, donde <em>disknumber</em> es el número de volumen y el que recibe el foco. Si no se especifica ningún volumen, el comando **select** muestra el volumen actual con el foco. Puedes especificar el volumen por número, letra de unidad o ruta de acceso de punto de montaje. En un disco básico, al seleccionar un volumen también recibe el foco de partición correspondiente.</p> |
| <p>**delete volume**</p>                     | <p>Elimina el volumen seleccionado. No puedes eliminar el volumen del sistema, el volumen de arranque o cualquier otro volumen que incluya el archivo de paginación activo o de volcado (volcado de memoria).</p> |
| <p>**convert basic**</p> | <p>Convierte un disco dinámico vacío en un disco básico.</p>  |

## <a name="additional-considerations"></a>Consideraciones adicionales

-   El disco no debe incluir ningún volumen o dato antes de cambiarlo por un disco básico. Si quieres conservar los datos, realiza una copia de seguridad o muévelos a otro volumen antes de convertir el disco en un disco básico.
-   Una vez que cambies un disco dinámico por un disco básico, solo puedes crear particiones y unidades lógicas en dicho disco.

## <a name="see-also"></a>Consulta también

-   [Notación de sintaxis de línea de comandos](https://technet.microsoft.com/library/cc742449(v=ws.11).aspx)


