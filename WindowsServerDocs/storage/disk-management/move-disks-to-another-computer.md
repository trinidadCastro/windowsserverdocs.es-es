---
title: Mover discos a otro equipo
description: En este artículo se describe cómo mover discos a otro equipo
ms.date: 10/12/2017
ms.prod: windows-server-threshold
ms.technology: storage
ms.topic: article
author: JasonGerend
manager: brianlic
ms.author: jgerend
ms.openlocfilehash: 6b235ce8e5b936940629d5977a17bbc729efbe82
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59854006"
---
# <a name="move-disks-to-another-computer"></a>Mover discos a otro equipo

> **Se aplica a:** Windows 10, Windows 8.1, Windows Server (canal semianual), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

En esta sección se describen los pasos a seguir y las consideraciones asociadas para mover discos a otro equipo. Es posible que quieras imprimir este procedimiento o anotar los pasos antes de intentar mover discos de un equipo a otro.

> [!NOTE]
> Debes ser miembro del grupo **Operadores de copia de seguridad** o **Administradores**, como mínimo, para completar estos pasos.

## <a name="verify-volume-health"></a>Comprobar el estado del volumen

Usa la Administración de discos para asegurarte de que el estado de los volúmenes en los discos sea **Correcto**. Si el estado no es **Correcto**, repara los volúmenes antes de mover los discos.

Para comprobar el estado del volumen, desde el menú **Vista**, comprueba la columna **Estado** en la vista **Lista de volúmenes**, o en el **tamaño de volumen** e información del sistema de archivos en la **Vista gráfica**.

## <a name="uninstall-the-disks"></a>Desinstalar los discos

Desinstala los discos que quieras mover con el Administrador de dispositivos.

**Para desinstalar los discos**

1.  Abre el Administrador de dispositivos en la Administración de equipos.

2.  En la lista de dispositivos, haz doble clic en **Unidades de disco**.

3.  Haz clic con el botón derecho en los discos que quieres desinstalar y luego haz clic en **Desinstalar**.

4.  En el cuadro de diálogo **Confirmar la eliminación del dispositivo**, haga clic en **Aceptar**.

## <a name="remove-dynamic-disks"></a>Quitar discos dinámicos

1. Si los discos que quieres mover son dinámicos discos, en la Administración de discos, haz clic con el botón derecho en los discos que quieras mover y luego haz clic en **Quitar disco**.

2. Tras quitar los discos dinámicos o si estás moviendo discos básicos, ahora puedes desconectarlos físicamente. Si los discos son externos, ahora puedes desconectarlos del equipo. Si los discos son internos, apaga el equipo y luego quítalos físicamente.

## <a name="install-disks-in-the-new-computer"></a>Instalar discos en el equipo nuevo

1. Si los discos son externos, conéctalos al equipo. Si los discos son internos, asegúrate de que el equipo esté desactivado y luego instala físicamente los discos en ese equipo.

2. Inicia el equipo que incluye los discos que moviste y sigue las instrucciones indicadas en el cuadro de diálogo Nuevo hardware encontrado.

## <a name="detect-new-disks"></a>Detectar nuevos discos

1. Abre el nuevo equipo, abre Administración de discos. 
2. Haz clic en **Acción** y luego haz clic en **Volver a examinar los discos**.
3. Haz clic con el botón derecho en cualquier disco marcado **Externo**. 
4. Haz clic en **Importar discos externos** y sigue las instrucciones en pantalla.

## <a name="additional-considerations"></a>Consideraciones adicionales

-   Cuando se mueve a otro equipo, los volúmenes básicos reciben la siguiente letra de unidad disponible en ese equipo. 
-   Los volúmenes dinámicos conservan la letra de unidad que tenían en el equipo anterior. Si un volumen dinámico no tenía una letra de unidad en el equipo anterior, no recibirá una letra de unidad cuando se mueve a otro equipo. Si la letra de unidad ya se usa en el equipo al que se mueve un volumen, el volumen recibe la siguiente letra de unidad disponible.

-   Si un administrador ha usado el comando **mountvol /n** o **diskpart automount** para evitar que se agreguen nuevos volúmenes al sistema, se evita montar y asignar una letra de unidad a los volúmenes movidos desde otro equipo. Para usar el volumen, debes montar el volumen manualmente y asignarle una letra de unidad con la Administración de discos o los comandos **DiskPart** y **mountvol**.

-   Si quieres mover volúmenes distribuidos, seccionados, de reflejo o RAID-5, se recomienda encarecidamente que muevas todos los discos que incluyan el volumen. De lo contrario, los volúmenes en los discos no se pueden poner en línea y no se podrán tener acceso a ellos salvo para eliminarlos.

-   Puedes mover varios discos desde equipos diferentes a un equipo si instalas los discos, abres la Administración de discos, haces clic con el botón derecho en cualquiera de los discos nuevos y luego haces clic en **Importar discos externos**. Al importar varios discos desde equipos diferentes, siempre debes importar todos los discos de un equipo a la vez. Por ejemplo, si quieres mover discos de dos equipos, importa todos los discos desde el primer equipo y luego importa todos los discos del segundo equipo.

-   La Administración de discos describe la condición de los volúmenes en los discos antes de importarlos. Revisa detenidamente esta información. Si hay algún problema, esta información te indicará qué sucederá con cada volumen de estos discos una vez que los discos se han importado.

-   Si mueves un disco de tabla de particiones GUID (GPT) que incluye el sistema operativo Windows en un equipo basado en x86 o x64, puedes tener acceso a los datos, pero no puedes arrancar desde ese sistema operativo.