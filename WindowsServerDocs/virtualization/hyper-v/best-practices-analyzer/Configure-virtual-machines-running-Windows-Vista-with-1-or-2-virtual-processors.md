---
title: Configuración de máquinas virtuales que ejecutan Windows Vista con 1 o 2 procesadores virtuales
description: Versión en línea del texto de esta regla de Analizador de procedimientos recomendados.
manager: dongill
ms.author: kathydav
ms.topic: article
ms.assetid: e562bce3-fd68-42c9-821c-12022ae4746c
author: kbdazure
ms.date: 8/16/2016
ms.openlocfilehash: 14be82374fe43314bc7e7fe95aaa00f774295ed6
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/07/2020
ms.locfileid: "87960648"
---
# <a name="configure-virtual-machines-running-windows-vista-with-1-or-2-virtual-processors"></a>Configuración de máquinas virtuales que ejecutan Windows Vista con 1 o 2 procesadores virtuales

>Se aplica a: Windows Server 2016

Para más información sobre los análisis y los procedimientos recomendados, vea [Ejecución de análisis del Analizador de procedimientos recomendados y administración de los resultados de los análisis](https://go.microsoft.com/fwlink/p/?LinkID=223177).

|Propiedad|Detalles|
|-|-|
|**Sistema operativo**|Windows Server 2016|
|**Producto/Característica**|Hyper-V|
|**Gravedad**|Configuración|
|**Categoría**|Error|

En las secciones siguientes, cursiva indica el texto de la interfaz de usuario que aparece en la herramienta de Analizador de procedimientos recomendados para este problema.

## <a name="issue"></a>Incidencia

*Una máquina virtual que ejecuta Windows Vista está configurada con más de dos procesadores virtuales.*

## <a name="impact"></a>Impacto

*Microsoft no admite la configuración de las siguientes máquinas virtuales:*

\<list of virtual machine names>

## <a name="resolution"></a>Resolución

*Apague la máquina virtual y Quite uno o varios procesadores virtuales.*

### <a name="to-remove-virtual-processors"></a>Para quitar procesadores virtuales

1.  Abra el administrador de Hyper-V. Haga clic en **Inicio**, seleccione **Herramientas administrativas** y, a continuación, haga clic en **Administrador de Hyper-V**.

2.  En el panel de resultados, en **virtual machines**, seleccione la máquina virtual que desea configurar. El estado de la máquina virtual debe aparecer como **desactivado**. Si no es así, haga clic con el botón secundario en la máquina virtual y, a continuación, haga clic en **apagar**.

3.  En el panel **Acción**, en el nombre de la máquina virtual, haga clic en **Configuración**.

4.  En el panel de navegación, haga clic en **procesador**.

5.  En la página **procesador** , establezca el número de procesadores en **1** o **2** y, a continuación, haga clic en **Aceptar**.



