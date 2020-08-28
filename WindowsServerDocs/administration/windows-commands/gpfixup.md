---
title: gpfixup
description: Artículo de referencia para el comando gpfixup, que corrige las dependencias de nombre de dominio en directiva de grupo objetos y vínculos de directiva de grupo después de una operación de cambio de nombre de dominio.
ms.topic: reference
ms.assetid: 2b145410-fc75-4526-932d-f16b7ee3aaef
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 6b11ae09bcc345c9fb656a8945e0febe5f4098ac
ms.sourcegitcommit: 96d46c702e7a9c3a321bbbb5284f73911c7baa3c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/27/2020
ms.locfileid: "89025699"
---
# <a name="gpfixup"></a>gpfixup

Corrige las dependencias de nombre de dominio en directiva de grupo objetos y vínculos de directiva de grupo después de una operación de cambio de nombre de dominio. Para usar este comando, debe instalar la administración de directiva de grupo como una característica a través de Administrador del servidor.

## <a name="syntax"></a>Sintaxis

```
gpfixup [/v]
[/olddns:<olddnsname> /newdns:<newdnsname>]
[/oldnb:<oldflatname> /newnb:<newflatname>]
[/dc:<dcname>] [/sionly]
[/user:<username> [/pwd:{<password>|*}]] [/?]
```

### <a name="parameters"></a>Parámetros

| Parámetro | Descripción |
| --------- |------------ |
| /v | Muestra mensajes de estado detallados. Si no se usa este parámetro, solo se mostrarán los mensajes de error o un mensaje de estado de resumen que indique, **Success** o **Failure** . |
| /olddns:`<olddnsname>` | Especifica el nombre DNS anterior del dominio cuyo nombre ha cambiado como `<olddnsname>` cuando la operación de cambio de nombre de dominio cambia el nombre DNS de un dominio. Puede usar este parámetro solo si también usa el parámetro **/newdns** para especificar un nuevo nombre DNS de dominio. |
| /newdns:`<newdnsname>` | Especifica el nuevo nombre DNS del dominio cuyo nombre ha cambiado como `<newdnsname>` cuando la operación de cambio de nombre de dominio cambia el nombre DNS de un dominio. Solo puede utilizar este parámetro si también usa el parámetro **/olddns** para especificar el nombre DNS del dominio anterior. |
| /oldnb:`<oldflatname>` | Especifica el nombre NetBIOS anterior del dominio cuyo nombre ha cambiado como `<oldflatname>` cuando la operación de cambio de nombre de dominio cambia el nombre NetBIOS de un dominio. Puede usar este parámetro solo si usa el parámetro **/newnb** para especificar un nuevo nombre de NetBIOS de dominio. |
| /newnb:`<newflatname>` | Especifica el nuevo nombre NetBIOS del dominio cuyo nombre ha cambiado como `<newflatname>` cuando la operación de cambio de nombre de dominio cambia el nombre NetBIOS de un dominio. Puede usar este parámetro solo si usa el parámetro **/oldnb** para especificar el nombre NetBIOS del dominio anterior. |
| /DC`<dcname>` | Conéctese al controlador de dominio denominado `<dcname>` (nombre DNS o nombre NetBIOS). `<dcname>` debe hospedar una réplica de escritura de la partición de directorio de dominio como se indica en una de las siguientes opciones:<ul><li>El nombre DNS mediante `<newdnsname>` **/newdns**</li><li>El nombre de NetBIOS mediante `<newflatname>` **/newnb**</br>Si no se usa este parámetro, puede conectarse a cualquier controlador de dominio en el dominio al que se ha cambiado el nombre indicado por `<newdnsname>` o `<newflatname>` .</li></ul> |
| /sionly | Realiza solo la directiva de grupo corrección relacionada con la instalación de software administrado (la extensión de instalación de software para directiva de grupo). Omitir las acciones que corrigen directiva de grupo vínculos y las rutas de acceso de SYSVOL en los GPO. |
| /User`<username>` |Ejecuta este comando en el contexto de seguridad del usuario `<username>` , donde `<username>` tiene el formato dominio\usuario. Si no se usa este parámetro, este comando se ejecuta como el usuario que ha iniciado sesión. |
| /pwd`{<password> | *}` | Especifica la contraseña del usuario. |
| /? | Muestra la Ayuda en el símbolo del sistema. |

### <a name="examples"></a>Ejemplos

En este ejemplo se da por supuesto que ya ha realizado una operación de cambio de nombre de dominio en la que ha cambiado el nombre DNS de **MyOldDnsName** a **MyNewDnsName**y el nombre de NetBIOS de **MyOldNetBIOSName** a **MyNewNetBIOSName**.

En este ejemplo, use el comando **gpfixup** para conectarse al controlador de dominio denominado **MyDcDnsName** y reparar los GPO y los vínculos de directiva de grupo actualizando el nombre de dominio anterior incrustado en los GPO y vínculos. El estado y la salida de error se guardan en un archivo denominado **gpfixup. log**.

```
gpfixup /olddns: MyOldDnsName /newdns:MyNewDnsName /oldnb:MyOldNetBIOSName /newnb:MyNewNetBIOSName /dc:MyDcDnsName 2>&1 >gpfixup.log
```

Este ejemplo es el mismo que el anterior, salvo que supone que el nombre NetBIOS del dominio no se ha cambiado durante la operación de cambio de nombre de dominio.

```
gpfixup /olddns: MyOldDnsName /newdns:MyNewDnsName /dc:MyDcDnsName 2>&1 >gpfixup.log
```

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)

- [Administrar Dominio de Active Directory cambiar el nombre](/previous-versions/windows/it-pro/windows-server-2008-r2-and-2008/cc794869(v=ws.10))
