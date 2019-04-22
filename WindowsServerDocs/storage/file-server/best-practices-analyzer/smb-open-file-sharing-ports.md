---
Title: 'SMB: Deben abrirse los puertos de compartir archivos e impresoras'
TOCTitle: 'SMB: File and printer sharing ports should be open'
ms.date: 07/02/2012
ms.prod: windows-server-threshold
ms.technology: storage
author: JasonGerend
manager: elizapo
ms.author: jgerend
ms.openlocfilehash: b6ad1f1f8573fc380e999e5ec2091cea8ebb8aa1
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59820366"
---
# <a name="smb-file-and-printer-sharing-ports-should-be-open"></a>SMB: Deben abrirse los puertos de compartir archivos e impresoras


Actualizado: 2 de febrero de 2011

Se aplica a: Windows Server 2019, Windows Server 2016, Windows Server 2012 R2 y Windows Server 2012, Windows Server 2008 R2

*En este tema está pensado para abordar un problema específico identificado por un análisis del analizador de procedimientos recomendados. Debe aplicar la información de este tema únicamente a los equipos que han tenido la ejecutó el archivo Services Best Practices Analyzer y están experimentando el problema mencionado en este tema. Para obtener más información sobre análisis y los procedimientos recomendados, consulte* [Best Practices Analyzer](http://go.microsoft.com/fwlink/?linkid=122786%0d%0a).


<table>
<colgroup>
<col style="width: 50%" />
<col style="width: 50%" />
</colgroup>
<tbody>
<tr class="odd">
<td><p><strong>Sistema operativo</strong></p></td>
<td><p>Windows Server</p></td>
</tr>
<tr class="even">
<td><p><strong>Característica del producto</strong></p></td>
<td><p>Servicios de archivos</p></td>
</tr>
<tr class="odd">
<td><p><strong>Gravedad</strong></p></td>
<td><p>Error</p></td>
</tr>
<tr class="even">
<td><p><strong>Categoría</strong></p></td>
<td><p>Configuración</p></td>
</tr>
</tbody>
</table>

## <a name="issue"></a>Problema

> *Los puertos de firewall son necesarios para compartir archivos e impresoras no abren (puertos 445 y 139).*

## <a name="impact"></a>Impacto

> *Los equipos no podrán tener acceso a las carpetas compartidas y otros servicios de red basada en bloque de mensajes del servidor SMB en este servidor.*

## <a name="resolution"></a>Resolución

> *Habilite Compartir archivos e impresoras para comunicarse a través del firewall del equipo.*

Para completar este procedimiento, se requiere como mínimo pertenecer al grupo **Administradores** o uno equivalente.

## <a name="to-open-the-firewall-ports-to-enable-file-and-printer-sharing"></a>Para abrir los puertos de firewall para permitir compartir archivos e impresoras

1.  Abra el Panel de Control, haga clic en **sistema y seguridad**y, a continuación, haga clic en **Windows Firewall**.

2.  En el panel izquierdo, haga clic en **configuración avanzada**y en el árbol de consola, haga clic en **reglas de entrada**.

3.  En **reglas de entrada**, busque las reglas **compartir archivos e impresoras (sesión NB de entrada)** y **compartir archivos e impresoras (SMB de entrada)**.

4.  Para cada regla, haga clic en la regla y, a continuación, haga clic en **Habilitar regla**.

## <a name="additional-references"></a>Referencias adicionales

[Comprender las carpetas compartidas y el Firewall de Windows](http://technet.microsoft.com/en-us/library/cc731402.aspx)()http://technet.microsoft.com/en-us/library/cc731402.aspx)

