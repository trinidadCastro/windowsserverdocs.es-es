---
description: 'Más información acerca de: SMB: los puertos de uso compartido de archivos e impresoras deben estar abiertos'
title: 'SMB: los puertos de uso compartido de impresoras y archivos deben estar abiertos'
ms.date: 07/02/2012
author: JasonGerend
manager: elizapo
ms.author: jgerend
ms.openlocfilehash: ca4e8cb86e567786385da0449223b6282347909b
ms.sourcegitcommit: 65b6de6b44d41f1180c45db11cdd60cb2a093b46
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/10/2020
ms.locfileid: "97041063"
---
# <a name="smb-file-and-printer-sharing-ports-should-be-open"></a>SMB: Los puertos de compartir archivos e impresoras deben estar abiertos


Actualizado: 2 de febrero de 2011

Se aplica a: Windows Server 2019, Windows Server 2016, Windows Server 2012 R2 y Windows Server 2012, Windows Server 2008 R2

*El objetivo de este tema es resolver un problema específico identificado por un análisis de analizador de procedimientos recomendados. Debe aplicar la información de este tema únicamente a los equipos que tienen los servicios de archivo Analizador de procedimientos recomendados ejecutarse en ellos y que experimentan el problema que se trata en este tema. Para obtener más información sobre los análisis y los procedimientos recomendados, consulte* [analizador de procedimientos recomendados](https://go.microsoft.com/fwlink/?linkid=122786%0d%0a).


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
<td><p><strong>Producto/Característica</strong></p></td>
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

> *Los puertos de Firewall necesarios para compartir archivos e impresoras no están abiertos (puertos 445 y 139).*

## <a name="impact"></a>Impacto

> *Los equipos no podrán tener acceso a las carpetas compartidas y a otros servicios de red basados en bloque de mensajes del servidor (SMB) en este servidor.*

## <a name="resolution"></a>Solución

> *Habilite el uso compartido de archivos e impresoras para comunicarse a través del firewall del equipo.*

Para completar este procedimiento, se requiere como mínimo pertenecer al grupo **Administradores** o uno equivalente.

## <a name="to-open-the-firewall-ports-to-enable-file-and-printer-sharing"></a>Para abrir los puertos de Firewall para habilitar el uso compartido de archivos e impresoras

1.  Abra el panel de control, haga clic en **sistema y seguridad** y, a continuación, haga clic en **firewall de Windows**.

2.  En el panel izquierdo, haga clic en **Configuración avanzada** y, en el árbol de consola, haga clic en **reglas de entrada**.

3.  En **reglas de entrada**, busque las reglas **compartir archivos e impresoras (sesión NB)** e **compartir impresoras y archivos (SMB de entrada)**.

4.  Para cada regla, haga clic con el botón derecho en la regla y, a continuación, haga clic en **Habilitar regla**.

## <a name="additional-references"></a>Referencias adicionales

[Descripción de las carpetas compartidas y el Firewall de Windows](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc731402(v=ws.11))(https://technet.microsoft.com/library/cc731402.aspx)
