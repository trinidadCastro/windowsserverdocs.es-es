---
title: Manage-BDE TPM
description: Tema de referencia de * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 11a8530d-edd7-4fe3-ae81-b943766760fe
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: afb3859a1795959c90e71391b2926164165ef9ce
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/01/2020
ms.locfileid: "82724072"
---
# <a name="manage-bde-tpm"></a>Manage-BDE: TPM

> Se aplica a: Windows Server (canal semianual), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2 y Windows Server 2012
> 
> [!IMPORTANT]
> Este comando no se admite para su uso en equipos que ejecutan Windows 8, Windows Server 2012 o sistemas operativos posteriores. Para esos equipos, puede usar los [cmdlets de administración de TPM para Windows PowerShell](https://docs.microsoft.com/powershell/module/trustedplatformmodule/).
> Si usa este comando en un equipo que ejecuta Windows 7 o Windows Server 2008, todavía puede configurar el Módulo de plataforma segura del equipo (TPM) con este comando.
> ## <a name="syntax"></a>Sintaxis
> ```
> manage-bde -tpm [-turnon] [-takeownership <OwnerPassword>] [-computername <Name>] [{-?|/?}] [{-help|-h}]
> ```
> #### <a name="parameters"></a>Parámetros
> 
> |    Parámetro    |                                                                              Descripción                                                                               |
> |-----------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
> |     -activando     |              Habilita y activa el TPM, lo que permite establecer la contraseña de propietario de TPM. También puede usar **-t** como una versión abreviada de este comando.              |
> | -takeownership  |                      Toma posesión del TPM mediante el establecimiento de una contraseña de propietario. También puede usar **-o** como una versión abreviada de este comando.                       |
> | <OwnerPassword> |                                                      Representa la contraseña de propietario que se especifica para el TPM.                                                       |
> |  -COMPUTERNAME  | Especifica que Manage-Bde. exe se usará para modificar la protección de BitLocker en otro equipo. También puede usar **-CN** como una versión abreviada de este comando. |
> |     <Name>      |    Representa el nombre del equipo en el que se va a modificar la protección de BitLocker. Los valores aceptados incluyen el nombre NetBIOS del equipo y la dirección IP del equipo.     |
> |    -? o/?     |                                                               Muestra una breve ayuda en el símbolo del sistema.                                                               |
> |   -Help o-h   |                                                             Muestra la ayuda completa en el símbolo del sistema.                                                              |
> 
> ## <a name="examples"></a>Ejemplos
> Ilustra el uso del comando **-TPM** para activar el TPM.
> ```
> manage-bde  tpm -turnon
> ```
> Ilustra el uso del comando **TPM** para tomar posesión del TPM y establecer la contraseña del propietario en 0wnerP@ss.
> ```
> manage-bde  tpm  takeownership 0wnerP@ss
> ```
> ## <a name="additional-references"></a>Referencias adicionales
> -   - [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)
> -   [manage-bde](manage-bde.md)
