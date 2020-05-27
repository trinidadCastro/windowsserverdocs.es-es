---
title: ksetup addkdc
description: Tema de referencia del comando ksetup addkdc, que incluye una dirección Centro de distribución de claves (KDC) para el dominio Kerberos determinado.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 98bfc23a-14c4-401c-bcb3-9903c5cdde64
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: e51279166bf60196d12f877506d3228b78c4a711
ms.sourcegitcommit: 4f407b82435afe3111c215510b0ef797863f9cb4
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/24/2020
ms.locfileid: "83818095"
---
# <a name="ksetup-addkdc"></a>ksetup addkdc

Agrega una dirección Centro de distribución de claves (KDC) para el dominio Kerberos determinado.

La asignación se almacena en el registro, en **HKEY_LOCAL_MACHINE \system\currentcontrolset\control\lsa\kerberos\domains** y el equipo debe reiniciarse antes de que se use la nueva configuración de dominio Kerberos.

> [!NOTE]
> Para implementar datos de configuración de dominio Kerberos en varios equipos, debe usar el complemento de **plantilla de configuración de seguridad** y la distribución de directivas, explícitamente en equipos individuales. No puede usar este comando.

## <a name="syntax"></a>Sintaxis

```
ksetup /addkdc <realmname> [<KDCname>]
```

### <a name="parameters"></a>Parámetros

| Parámetro | Descripción |
| --------- | ----------- |
| `<realmname>` | Especifica el nombre DNS en mayúsculas, como CORP. CONTOSO.COM. Este valor también aparece como el dominio Kerberos predeterminado cuando se ejecuta **ksetup** y es el dominio al que desea agregar el otro KDC. |
| `<KDCname>` | Especifica el nombre de dominio completo que no distingue entre mayúsculas y minúsculas, como mitkdc.contoso.com. Si se omite el nombre del KDC, DNS buscará KDC. |

### <a name="examples"></a>Ejemplos

Para configurar un servidor KDC que no sea de Windows y el dominio Kerberos que debe usar la estación de trabajo, escriba:

```
ksetup /addkdc CORP.CONTOSO.COM mitkdc.contoso.com
```

Para establecer la contraseña de la cuenta de equipo local en p@sswrd1 % en el mismo equipo que en el ejemplo anterior y, a continuación, para reiniciar el equipo, escriba:

```
ksetup /setcomputerpassword p@sswrd1%
```

Para comprobar el nombre de dominio Kerberos predeterminado para el equipo o para comprobar que este comando ha funcionado según lo previsto, escriba:

```
ksetup
```
Compruebe el registro para asegurarse de que la asignación se produjo según lo previsto.

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)

- [ksetup, comando](ksetup.md)

- [ksetup setcomputerpassword, comando](ksetup-setcomputerpassword.md)
