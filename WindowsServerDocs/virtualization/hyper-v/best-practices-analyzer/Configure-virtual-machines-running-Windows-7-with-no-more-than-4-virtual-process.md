---
title: Configuración de máquinas virtuales que ejecutan Windows 7 con un máximo de 4 procesadores virtuales
description: Obtenga información acerca de qué hacer cuando una máquina virtual que ejecuta Windows 7 está configurada con más de 4 procesadores virtuales.
ms.author: benarm
author: BenjaminArmstrong
ms.topic: article
ms.assetid: 8fcf0868-b543-4f94-aee7-35324346da55
ms.date: 8/16/2016
ms.openlocfilehash: f52884ea6ce7298980f42b1aca876866c7dbaa77
ms.sourcegitcommit: 42581433c0bb62e291d412ee9e13869b42e69a4b
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/01/2021
ms.locfileid: "97846398"
---
# <a name="configure-virtual-machines-running-windows-7-with-no-more-than-4-virtual-processors"></a>Configuración de máquinas virtuales que ejecutan Windows 7 con un máximo de 4 procesadores virtuales

>Se aplica a: Windows Server 2016

Para más información sobre los análisis y los procedimientos recomendados, vea [Ejecución de análisis del Analizador de procedimientos recomendados y administración de los resultados de los análisis](https://go.microsoft.com/fwlink/p/?LinkID=223177).

|Propiedad|Detalles|
|-|-|
|**Sistema operativo**|Windows Server 2016|
|**Producto/Característica**|Hyper-V|
|**Gravedad**|Error|
|**Categoría**|Configuración|

En las secciones siguientes, cursiva indica el texto de la interfaz de usuario que aparece en la herramienta de Analizador de procedimientos recomendados para este problema.

## <a name="issue"></a>**Problema**
*Una máquina virtual que ejecuta Windows 7 está configurada con más de 4 procesadores virtuales.*

## <a name="impact"></a>**Impacto**
*Microsoft no admite la configuración de las siguientes máquinas virtuales:*

\<list of virtual machines>

## <a name="resolution"></a>**Resolución**
*Apague la máquina virtual y Quite uno o varios procesadores virtuales.*

#### <a name="to-remove-virtual-processors"></a>Para quitar procesadores virtuales

1.  Abra el administrador de Hyper-V. Haga clic en **Inicio**, seleccione **Herramientas administrativas** y, a continuación, haga clic en **Administrador de Hyper-V**.

2.  En el panel de resultados, en **virtual machines**, seleccione la máquina virtual que desea configurar. El estado de la máquina virtual debe aparecer como **desactivado**. Si no es así, haga clic con el botón secundario en la máquina virtual y, a continuación, haga clic en **apagar**.

3.  En el panel **Acción**, en el nombre de la máquina virtual, haga clic en **Configuración**.

4.  En el panel de navegación, haga clic en **procesador**.

5.  En la página **procesador** , establezca el número de procesadores en **3** o menos y, a continuación, haga clic en **Aceptar**.



