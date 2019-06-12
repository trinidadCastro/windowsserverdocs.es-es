---
title: ¿Manage-bde tpm
description: 'Tema de los comandos de Windows para ***- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 11a8530d-edd7-4fe3-ae81-b943766760fe
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 0d41c846034ad421d0da81bda57acbcd419c1ae1
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/31/2019
ms.locfileid: "66437393"
---
# <a name="manage-bde-tpm"></a>¿Manage-bde: tpm

> Se aplica a: Windows Server (canal semianual), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012
> 
> [!IMPORTANT]
> Este comando no se admite para su uso en equipos que ejecutan Windows 8, Windows Server 2012 o sistemas operativos posteriores. Para esos equipos, puede usar el [cmdlets de administración de TPM de Windows PowerShell](https://docs.microsoft.com/en-us/powershell/module/trustedplatformmodule/).
> Si usa este comando en el equipo que ejecuta Windows 7 o Windows Server 2008, puede configurar plataforma módulo segura (TPM del equipo) con este comando. Para obtener ejemplos de cómo se puede usar este comando, consulte [ejemplos](#BKMK_Examples).
> ## <a name="syntax"></a>Sintaxis
> ```
> manage-bde -tpm [-turnon] [-takeownership <OwnerPassword>] [-computername <Name>] [{-?|/?}] [{-help|-h}]
> ```
> ### <a name="parameters"></a>Parámetros
> 
> |    Parámetro    |                                                                              Descripción                                                                               |
> |-----------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
> |     -prender     |              Habilita y activa el TPM, lo que permite la contraseña de propietario TPM debe establecerse. También puede usar **-t** como una versión abreviada de este comando.              |
> | -takeownership  |                      Toma posesión de TPM mediante el establecimiento de una contraseña de propietario. También puede usar **-o** como una versión abreviada de este comando.                       |
> | <OwnerPassword> |                                                      Representa la contraseña de propietario que especifique para el TPM.                                                       |
> |  -computername  | Especifica que se usará ese administrar-bde.exe para modificar la protección de BitLocker en un equipo diferente. También puede usar **- cn** como una versión abreviada de este comando. |
> |     <Name>      |    Representa el nombre del equipo en el que se va a modificar la protección de BitLocker. Valores aceptados incluyen el nombre NetBIOS del equipo y dirección IP del equipo.     |
> |    -? ¿o /?     |                                                               Muestra una breve ayuda en el símbolo del sistema.                                                               |
> |   -help o -h   |                                                             Muestra Ayuda en la línea de comandos completa.                                                              |
> 
> ## <a name="BKMK_Examples"></a>Ejemplos
> El ejemplo siguiente se muestra cómo utilizar el **- tpm** comando para activar TPM.
> ```
> manage-bde  tpm -turnon
> ```
> El ejemplo siguiente se muestra cómo utilizar el **tpm** comando para tomar posesión de TPM y establecer la contraseña de propietario en 0wnerP@ss.
> ```
> manage-bde  tpm  takeownership 0wnerP@ss
> ```
> ## <a name="additional-references"></a>Referencias adicionales
> -   [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)
> -   [manage-bde](manage-bde.md)
