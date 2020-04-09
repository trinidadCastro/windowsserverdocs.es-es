---
title: Manage-BDE TPM
description: Tema de comandos de Windows para * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 11a8530d-edd7-4fe3-ae81-b943766760fe
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: b6495bfbfedea7219ae175145f72fc12314ce7ae
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/08/2020
ms.locfileid: "80839768"
---
# <a name="manage-bde-tpm"></a>Manage-BDE: TPM

> Se aplica a: Windows Server (canal semianual), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012
> 
> [!IMPORTANT]
> Este comando no se admite para su uso en equipos que ejecutan Windows 8, Windows Server 2012 o sistemas operativos posteriores. Para esos equipos, puede usar los [cmdlets de administración de TPM para Windows PowerShell](https://docs.microsoft.com/powershell/module/trustedplatformmodule/).
> Si usa este comando en un equipo que ejecuta Windows 7 o Windows Server 2008, todavía puede configurar el Módulo de plataforma segura del equipo (TPM) con este comando. Para obtener ejemplos de cómo se puede usar este comando, vea [ejemplos](#BKMK_Examples).
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
> ## <a name="examples"></a><a name=BKMK_Examples></a>Example
> En el siguiente ejemplo se muestra el uso del comando **-TPM** para activar el TPM.
> ```
> manage-bde  tpm -turnon
> ```
> En el siguiente ejemplo se muestra el uso del comando **TPM** para tomar posesión del TPM y establecer la contraseña del propietario en 0wnerP@ss.
> ```
> manage-bde  tpm  takeownership 0wnerP@ss
> ```
> ## <a name="additional-references"></a>Referencias adicionales
> -   - [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)
> -   [manage-bde](manage-bde.md)
