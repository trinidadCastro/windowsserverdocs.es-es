---
title: gpfixup
description: Tema de comandos de Windows para * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 2b145410-fc75-4526-932d-f16b7ee3aaef
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: e0f1ce2e050a3d741f9b2f7a5cce9d2988716106
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/08/2020
ms.locfileid: "80842558"
---
# <a name="gpfixup"></a>gpfixup



Corrija las dependencias de nombre de dominio en directiva de grupo objetos y vínculos de directiva de grupo después de una operación de cambio de nombre de dominio. Para obtener ejemplos de cómo se puede usar este comando, vea [ejemplos](#BKMK_Examples).

## <a name="syntax"></a>Sintaxis

```
Gpfixup [/v] 
[/olddns:<OLDDNSNAME> /newdns:<NEWDNSNAME>] 
[/oldnb:<OLDFLATNAME> /newnb:<NEWFLATNAME>] 
[/dc:<DCNAME>] [/sionly] 
[/user:<USERNAME> [/pwd:{<PASSWORD>|*}]] [/?]
```

#### <a name="parameters"></a>Parámetros

|       Parámetro       |                                                                                                                                                                                                                               Descripción                                                                                                                                                                                                                               |
|-----------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|          /v           |                                                                                                                                                      Muestra mensajes de estado detallados.</br>Si no se usa este parámetro, solo se mostrarán los mensajes de error o el mensaje de Resumen de estado **correcto** o **error** .                                                                                                                                                       |
| /olddns:\<OLDDNSNAME > |                                                                                                           Especifica el nombre DNS anterior del dominio cuyo nombre ha cambiado como *\<OLDDNSNAME >* cuando la operación de cambio de nombre de dominio cambia el nombre DNS de un dominio. Puede usar este parámetro solo si también usa el parámetro **/newdns** para especificar un nuevo nombre DNS de dominio.                                                                                                            |
| /newdns:\<NEWDNSNAME > |                                                                                                          Especifica el nuevo nombre DNS del dominio cuyo nombre ha cambiado como *\<NEWDNSNAME >* cuando la operación de cambio de nombre de dominio cambia el nombre DNS de un dominio. Solo puede utilizar este parámetro si también usa el parámetro **/olddns** para especificar el nombre DNS del dominio anterior.                                                                                                           |
| /oldnb:\<OLDFLATNAME > |                                                                                                        Especifica el nombre NetBIOS anterior del dominio cuyo nombre ha cambiado como *\<OLDFLATNAME >* cuando la operación de cambio de nombre de dominio cambia el nombre NetBIOS de un dominio. Puede usar este parámetro solo si usa el parámetro **/newnb** para especificar un nuevo nombre de NetBIOS de dominio.                                                                                                        |
| /newnb:\<NEWFLATNAME > |                                                                                                       Especifica el nuevo nombre NetBIOS del dominio cuyo nombre ha cambiado como *\<NEWFLATNAME >* cuando la operación de cambio de nombre de dominio cambia el nombre NetBIOS de un dominio. Puede usar este parámetro solo si usa el parámetro **/oldnb** para especificar el nombre NetBIOS del dominio anterior.                                                                                                       |
|     /DC:\<NOMBREDEDC >     | Conéctese al controlador de dominio llamado *\<nombrededc >* (un nombre DNS o un nombre NetBIOS). *\<nombrededc >* debe hospedar una réplica de escritura de la partición de directorio de dominio, como se indica en una de las siguientes opciones:</br>-El nombre DNS *\<NEWDNSNAME >* mediante **/newdns**</br>-El nombre de NetBIOS *\<NEWFLATNAME >* mediante **/newnb**</br>Si no se usa este parámetro, conéctese a cualquier controlador de dominio en el dominio al que se ha cambiado el nombre indicado por *\<NEWDNSNAME >* o *\<NEWFLATNAME >* . |
|        /sionly        |                                                                                                                           Realiza solo la directiva de grupo corrección relacionada con la instalación de software administrado (la extensión de instalación de software para directiva de grupo). Omitir las acciones que corrigen directiva de grupo vínculos y las rutas de acceso de SYSVOL en los GPO.                                                                                                                           |
|   /User:\<nombre de usuario >   |                                                                                                                                   Ejecuta este comando en el contexto de seguridad del usuario *\<username >* , donde *\<username >* tiene el formato dominio\usuario.</br>Si no se usa este parámetro, ejecuta este comando como el usuario que ha iniciado sesión.                                                                                                                                    |
|   /PWD: {\<contraseña >   |                                                                                                                                                                                                                                   \*}                                                                                                                                                                                                                                   |
|          /?           |                                                                                                                                                                                                                  Muestra la Ayuda en el símbolo del sistema.                                                                                                                                                                                                                   |

## <a name="remarks"></a>Comentarios

-   El comando **gpfixup** está disponible en windows Server 2008 R2 y windows Server 2008, excepto en las instalaciones Server Core.
-   Aunque el Consola de administración de directivas de grupo (GPMC) se distribuye con Windows Server 2008 R2 y Windows Server 2008, debe instalar la administración de directiva de grupo como una característica a través de Administrador del servidor.

## <a name="examples"></a><a name=BKMK_Examples></a>Example

En este ejemplo se da por supuesto que ya ha realizado una operación de cambio de nombre de dominio en la que ha cambiado el nombre DNS de **MyOldDnsName** a **MyNewDnsName**y el nombre de NetBIOS de **MyOldNetBIOSName** a **MyNewNetBIOSName**. En este ejemplo, use el comando **gpfixup** para conectarse al controlador de dominio denominado **MyDcDnsName** y reparar los GPO y los vínculos de directiva de grupo actualizando el nombre de dominio anterior incrustado en los GPO y vínculos. El estado y la salida de error se guardan en un archivo denominado **gpfixup. log**.
```
gpfixup /olddns: MyOldDnsName /newdns:MyNewDnsName /oldnb:MyOldNetBIOSName /newnb:MyNewNetBIOSName /dc:MyDcDnsName 2>&1 >gpfixup.log
```
Este ejemplo es el mismo que el anterior, salvo que supone que el nombre NetBIOS del dominio no se ha cambiado durante la operación de cambio de nombre de dominio.
```
gpfixup /olddns: MyOldDnsName /newdns:MyNewDnsName /dc:MyDcDnsName 2>&1 >gpfixup.log
```

## <a name="additional-references"></a>Referencias adicionales

-   [Administrar Dominio de Active Directory cambiar el nombre](https://go.microsoft.com/fwlink/?LinkId=198385)
-   [TechCenter de directiva de grupo](https://go.microsoft.com/fwlink/?LinkID=145531)
-   - [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)