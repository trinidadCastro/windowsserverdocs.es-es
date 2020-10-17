---
title: tsecimp
description: Artículo de referencia para el comando tsecimp, que importa la información de asignación de un archivo lenguaje de marcado extensible (XML) en el archivo de seguridad del servidor TAPI (Tsec.ini).
ms.topic: reference
ms.assetid: d7488ec6-0eff-45ff-89ee-9cbe752416bf
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: 0db7c7c191b4ca4cdd1d266892b68aa717c142da
ms.sourcegitcommit: f45640cf4fda621b71593c63517cfdb983d1dc6a
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/17/2020
ms.locfileid: "92156755"
---
# <a name="tsecimp"></a>tsecimp

Importa la información de asignación de un archivo lenguaje de marcado extensible (XML) en el archivo de seguridad del servidor TAPI (Tsec.ini). También puede usar este comando para mostrar la lista de proveedores TAPI y los dispositivos de línea asociados a cada uno de ellos, validar la estructura del archivo XML sin importar el contenido y comprobar la pertenencia al dominio.

## <a name="syntax"></a>Sintaxis

```
tsecimp /f <filename> [{/v | /u}]
tsecimp /d
```

### <a name="parameters"></a>Parámetros

| Parámetro | Descripción |
|--|--|
| /f `<filename>` | Necesario. Especifica el nombre del archivo XML que contiene la información de asignaciones que desea importar. |
| /v | Valida la estructura del archivo XML sin importar la información al archivo Tsec.ini. |
| /U | Comprueba si cada usuario pertenece al dominio especificado en el archivo XML. El equipo donde use este parámetro deberá estar conectado a la red. Es posible que el parámetro reduzca considerablemente el rendimiento si se procesa una gran cantidad de información de asignaciones de usuarios. |
| /d | Muestra una lista de los proveedores de telefonía instalados. Para cada proveedor de telefonía, se enumeran los dispositivos de línea asociados, así como las direcciones y los usuarios asociados a cada dispositivo de línea. |
| /? | Muestra la ayuda en el símbolo del sistema. |

#### <a name="remarks"></a>Comentarios

El archivo XML desde el que desea importar la información de asignación debe seguir la estructura que se describe a continuación:

```xml
<UserList>
  <User>
    <LineList>
      <Line>
```

- `<Userlist element>` : El elemento superior del archivo XML.

- `<User element>` : Contiene información acerca de un usuario que es miembro de un dominio. Es posible asignar uno o varios dispositivos de línea a cada usuario. Además, cada elemento de **usuario** podría tener un atributo denominado **NoMerge**. Cuando se especifica este atributo, se quitan todas las asignaciones de dispositivos de línea del usuario antes de realizar nuevas asignaciones. Puede usar este atributo para quitar fácilmente las asignaciones de usuario no deseadas. De forma predeterminada, este atributo no está establecido. El elemento **User** debe contener un único elemento **DomainUserName** , que especifica el dominio y el nombre de usuario del usuario. El elemento **User** también puede contener un elemento **FriendlyName** , que especifica un nombre descriptivo para el usuario. El elemento **User** podría contener un elemento **LineList** . Si un elemento **LineList** no está presente, se quitan todos los dispositivos de línea de este usuario.

- `<LineList element>` : Contiene información sobre cada línea o dispositivo que se puede asignar al usuario. Cada elemento **LineList** puede contener más de un elemento de **línea** .

- `<Line element>` : Especifica un dispositivo de línea. Debe identificar cada dispositivo de línea agregando un elemento de **Dirección** o un elemento **PermanentID** en el elemento de **línea** . Para cada elemento de **línea** , puede establecer el atributo **Remove** . Si se establece este atributo, el dispositivo de línea deja de estar asignado al usuario. Si no se establece este atributo, el usuario obtiene acceso a dicho dispositivo de línea. No se proporciona ningún error si el dispositivo de línea no está disponible para el usuario.

##### <a name="sample-output-for-d-parameter"></a>Salida de ejemplo para el parámetro/d

Esta salida de ejemplo aparece después de ejecutar el parámetro **/d** para mostrar la configuración actual de TAPI. Para cada proveedor de telefonía, se enumeran los dispositivos de línea asociados, así como las direcciones y los usuarios asociados a cada dispositivo de línea.

```
NDIS Proxy TAPI Service Provider
  Line: WAN Miniport (L2TP)
    Permanent ID: 12345678910

NDIS Proxy TAPI Service Provider
  Line: LPT1DOMAIN1\User1
    Permanent ID: 12345678910

Microsoft H.323 Telephony Service Provider
  Line: H323 Line
    Permanent ID: 123456
    Addresses:
      BLDG1-TAPI32
```

## <a name="examples"></a>Ejemplos

Para quitar todos los dispositivos de línea asignados a *user1*, escriba:

```xml
<UserList>
  <User NoMerge=1>
    <DomainUser>domain1\user1</DomainUser>
  </User>
</UserList>
```

Para quitar todos los dispositivos de línea asignados a *user1*, antes de asignar una línea con la dirección *99999*, escriba:

```xml
<UserList>
  <User NoMerge=1>
  <DomainUser>domain1\user1</DomainUser>
  <FriendlyName>User1</FriendlyName>
  <LineList>
    <Line>
      <Address>99999</Address>
    </Line>
  </LineList>
  </User>
</UserList>
```

En este ejemplo, *user1* no tiene ningún otro dispositivo de línea asignado, independientemente de si se asignó previamente algún dispositivo de línea.

Para agregar un dispositivo de línea para *user1*, sin eliminar los dispositivos de línea asignados previamente, escriba:

```xml
<UserList>
  <User>
  <DomainUser>domain1\user1</DomainUser>
  <FriendlyName>User1</FriendlyName>
  <LineList>
    <Line>
      <Address>99999</Address>
    </Line>
  </LineList>
  </User>
</UserList>
```

Para agregar la dirección de línea *99999* y quitar la dirección de línea *88888* del acceso de *usuario1* , escriba:

```xml
<UserList>
  <User>
  <DomainUser>domain1\user1</DomainUser>
  <FriendlyName>User1</FriendlyName>
  <LineList>
    <Line>
      <Address>99999</Address>
    </Line>
    <Line Remove=1>
      <Address>88888</Address>
    </Line>
  </LineList>
  </User>
</UserList>
```

Para agregar el dispositivo permanente *1000* y quitar la línea *88888* del acceso de *usuario1* , escriba:

```xml
<UserList>
  <User>
  <DomainUser>domain1\user1</DomainUser>
  <FriendlyName>User1</FriendlyName>
  <LineList>
    <Line>
    <PermanentID>1000</PermanentID>
    </Line>
    <Line Remove=1>
    <Address>88888</Address>
    </Line>
  </LineList>
  </User>
</UserList>
```

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)

- [Información general del shell de comandos](/previous-versions/windows/it-pro/windows-server-2003/cc737438(v=ws.10))
