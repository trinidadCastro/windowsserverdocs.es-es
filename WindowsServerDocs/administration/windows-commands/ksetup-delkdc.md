---
title: ksetup delkdc
description: Artículo de referencia para el comando ksetup delkdc, que elimina instancias de los nombres de Centro de distribución de claves (KDC) del dominio Kerberos.
ms.topic: reference
ms.assetid: 7d6ec389-094c-4a7b-a78b-605497ddc289
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: 6bdc53a77f822130553af9d88e18658227c899fa
ms.sourcegitcommit: db2d46842c68813d043738d6523f13d8454fc972
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/10/2020
ms.locfileid: "89639686"
---
# <a name="ksetup-delkdc"></a>ksetup delkdc

Elimina instancias de nombres de Centro de distribución de claves (KDC) para el dominio Kerberos.

La asignación se almacena en el registro, en `HKEY_LOCAL_MACHINE\System\CurrentControlSet\Control\LSA\Kerberos\Domains` . Después de ejecutar este comando, se recomienda asegurarse de que se ha quitado el KDC y que ya no aparece en la lista.

> [!NOTE]
> Para quitar los datos de configuración de dominio Kerberos de varios equipos, use el complemento de **plantilla de configuración de seguridad** con distribución de directivas, en lugar de usar el comando **ksetup** explícitamente en equipos individuales.

## <a name="syntax"></a>Sintaxis

```
ksetup /delkdc <realmname> <KDCname>
```

### <a name="parameters"></a>Parámetros

| Parámetro | Descripción |
| --------- | ----------- |
| `<realmname>` | Especifica el nombre DNS en mayúsculas, como CORP. CONTOSO.COM. Este es el dominio Kerberos predeterminado que aparece cuando se ejecuta el comando **ksetup** y es el dominio Kerberos del que se desea eliminar el KDC. |
| `<KDCname>` | Especifica el nombre de dominio completo que distingue entre mayúsculas y minúsculas, como mitkdc.contoso.com. |

### <a name="examples"></a>Ejemplos

Para ver todas las asociaciones entre el dominio de Windows y el dominio que no es de Windows y para determinar cuáles se van a quitar, escriba:

```
ksetup
```

Para quitar la asociación, escriba:

```
ksetup /delkdc CORP.CONTOSO.COM mitkdc.contoso.com
```

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)

- [ksetup, comando](ksetup.md)

- [ksetup addkdc, comando](ksetup-addkdc.md)