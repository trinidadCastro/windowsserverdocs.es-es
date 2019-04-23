---
title: gpfixup
description: 'Tema de los comandos de Windows para ***- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 2b145410-fc75-4526-932d-f16b7ee3aaef
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: fdbe9873cc15866037c4688aaac89095e4a85dec
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59889426"
---
# <a name="gpfixup"></a>gpfixup



Corregir las dependencias de nombre de dominio en los vínculos de objetos de directiva de grupo y directiva de grupo después de cambiar el nombre de un dominio de la operación. Para obtener ejemplos de cómo se puede usar este comando, consulte [ejemplos](#BKMK_Examples).

## <a name="syntax"></a>Sintaxis

```
Gpfixup [/v] 
[/olddns:<OLDDNSNAME> /newdns:<NEWDNSNAME>] 
[/oldnb:<OLDFLATNAME> /newnb:<NEWFLATNAME>] 
[/dc:<DCNAME>] [/sionly] 
[/user:<USERNAME> [/pwd:{<PASSWORD>|*}]] [/?]
```

### <a name="parameters"></a>Parámetros

|Parámetro|Descripción|
|---------|-----------|
|/v|Muestra los mensajes de estado detallado.</br>Si no se usa este parámetro, sólo los mensajes de error o un mensaje de estado de resumen de **éxito** o **error** aparece.|
|/olddns:\<OLDDNSNAME >|Especifica el nombre antiguo de DNS del dominio con el nombre cambiado como  *\<OLDDNSNAME >* cuando la operación de cambio de nombre de dominio cambia el nombre DNS de un dominio. Puede usar este parámetro solo si también usa el **/newdns** parámetro para especificar un nuevo nombre de dominio DNS.|
|/newdns:\<NEWDNSNAME >|Especifica el nuevo nombre DNS del dominio con el nombre cambiado como  *\<NEWDNSNAME >* cuando la operación de cambio de nombre de dominio cambia el nombre DNS de un dominio. Puede usar este parámetro solo si también usa el **/olddns** parámetro para especificar el nombre de dominio DNS anterior.|
|/oldnb:\<OLDFLATNAME >|Especifica el nombre antiguo de NetBIOS del dominio con el nombre cambiado como  *\<OLDFLATNAME >* cuando la operación de cambio de nombre de dominio cambia el nombre de NetBIOS de un dominio. Puede usar este parámetro solo si usa la **/newnb** parámetro para especificar un nuevo nombre de NetBIOS del dominio.|
|/newnb:\<NEWFLATNAME>|Especifica el nuevo nombre de NetBIOS del dominio con el nombre cambiado como  *\<NEWFLATNAME >* cuando la operación de cambio de nombre de dominio cambia el nombre de NetBIOS de un dominio. Puede usar este parámetro solo si usa la **/oldnb** parámetro para especificar el nombre NetBIOS del dominio antiguo.|
|/ DC:\<DCNAME >|Conectar con el controlador de dominio denominado  *\<DCNAME >* (un nombre DNS o un nombre NetBIOS). *\<DCNAME >* debe hospedar una réplica de escritura de la partición de directorio de dominio, como se indica mediante uno de los siguientes:</br>-El nombre DNS  *\<NEWDNSNAME >* utilizando **/newdns**</br>-El nombre de NetBIOS  *\<NEWFLATNAME >* utilizando **/newnb**</br>Si no se usa este parámetro, conectarse a cualquier controlador de dominio en el dominio ha cambiado el nombre indicado por  *\<NEWDNSNAME >* o  *\<NEWFLATNAME >*.|
|/sionly|Realiza sólo la corrección de la directiva de grupo relacionada con la instalación de software administrado (la extensión de instalación de Software de directiva de grupo). Omitir las acciones que corrigen los vínculos de directiva de grupo y las rutas de acceso SYSVOL en los GPO.|
|/ User:\<nombre de usuario >|Se ejecuta este comando en el contexto de seguridad del usuario  *\<USERNAME >*, donde  *\<USERNAME >* está en el formato dominio\usuario.</br>Si no se usa este parámetro, ejecuta este comando como el usuario que inició sesión.|
|/pwd:{\<PASSWORD>|*}|Especifica la contraseña para el otro contexto de seguridad indicada mediante el uso de **/user**. Si **&#42;** se especifica en lugar de una contraseña, se le solicitará una contraseña.|
|/?|Muestra la Ayuda en el símbolo del sistema.|

## <a name="remarks"></a>Comentarios

-   El **gpfixup** comando está disponible en Windows Server 2008 R2 y Windows Server 2008, excepto en las instalaciones Server Core.
-   Aunque la consola de administración de directivas de grupo (GPMC) se distribuye con Windows Server 2008 R2 y Windows Server 2008, debe instalar Administración de directivas de grupo como una característica mediante el administrador del servidor.

## <a name="BKMK_Examples"></a>Ejemplos

En este ejemplo se da por supuesto que ya ha realizado una operación de cambio de nombre de dominio en el que ha cambiado el nombre DNS de **MyOldDnsName** a **MyNewDnsName**y el nombre de NetBIOS de  **MyOldNetBIOSName** a **MyNewNetBIOSName**. En este ejemplo, se usa el **gpfixup** comando para conectarse al controlador de dominio denominado **MyDcDnsName** y reparar GPO y directivas de grupo vínculos al actualizar el nombre de dominio antiguo incrustarán en el GPO y vínculos. Salida de error y de estado se guarda en un archivo denominado **gpfixup.log**.
```
gpfixup /olddns: MyOldDnsName /newdns:MyNewDnsName /oldnb:MyOldNetBIOSName /newnb:MyNewNetBIOSName /dc:MyDcDnsName 2>&1 >gpfixup.log
```
Este ejemplo es el mismo que el anterior, excepto en que se supone que el nombre NetBIOS del dominio no se cambió durante el dominio de cambiar el nombre de operación.
```
gpfixup /olddns: MyOldDnsName /newdns:MyNewDnsName /dc:MyDcDnsName 2>&1 >gpfixup.log
```

#### <a name="additional-references"></a>Referencias adicionales

-   [Administración de cambio de nombre de dominio de Active Directory](https://go.microsoft.com/fwlink/?LinkId=198385)
-   [TechCenter de directiva de grupo](https://go.microsoft.com/fwlink/?LinkID=145531)
-   [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)