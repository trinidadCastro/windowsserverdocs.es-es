---
title: tsecimp
description: 'Tema de comandos de Windows para * * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: d7488ec6-0eff-45ff-89ee-9cbe752416bf
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 85fea84ed9dcb0f85bfa80e56f0c2c04d2c8e85b
ms.sourcegitcommit: 1bc3c229e9688ac741838005ec4b88e8f9533e8a
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/18/2019
ms.locfileid: "68314314"
---
# <a name="tsecimp"></a>tsecimp



Importa la información de asignación de un archivo lenguaje de marcado extensible (XML) en el archivo de seguridad del servidor TAPI (TSEC. ini). También puede usar este comando para mostrar la lista de proveedores TAPI y los dispositivos de línea asociados a cada uno de ellos, validar la estructura del archivo XML sin importar el contenido y comprobar la pertenencia al dominio.

## <a name="syntax"></a>Sintaxis

```
tsecimp /f <Filename> [{/v | /u}]
tsecimp /d
```

### <a name="parameters"></a>Parámetros

|Parámetro|Descripción|
|---------|-----------|
|/f \<nombre de archivo >|Necesario. Especifica el nombre del archivo XML que contiene la información de asignación que desea importar.|
|/v|Valida la estructura del archivo XML sin importar la información en el archivo TSEC. ini.|
|/u|Comprueba si cada usuario es miembro del dominio especificado en el archivo XML. El equipo en el que se usa este parámetro debe estar conectado a la red. Este parámetro puede reducir significativamente el rendimiento si se está procesando una gran cantidad de información de asignación de usuarios.|
|/d|Muestra una lista de proveedores de telefonía instalados. Para cada proveedor de telefonía, se enumeran los dispositivos de línea asociados, así como las direcciones y los usuarios asociados a cada dispositivo de línea.|
|/?|Muestra la ayuda en el símbolo del sistema.|

## <a name="remarks"></a>Comentarios

-   El archivo XML desde el que desea importar la información de asignación debe seguir la estructura que se describe a continuación.  
    -   Elemento **userList**

        **UserList** es el elemento superior del archivo XML.
    -   Elemento **User**

        Cada elemento de **usuario** contiene información acerca de un usuario que es miembro de un dominio. A cada usuario se le puede asignar uno o varios dispositivos de línea.

        Además, cada elemento de **usuario** podría tener un atributo denominado NoMerge. Cuando se especifica este atributo, se quitan todas las asignaciones de dispositivos de línea actuales para el usuario antes de que se realicen otras nuevas. Puede usar este atributo para quitar fácilmente las asignaciones de usuario no deseadas. De forma predeterminada, este atributo no está establecido.

        El elemento **User** debe contener un único elemento **DomainUserName** , que especifica el dominio y el nombre de usuario del usuario. El elemento **User** también puede contener un elemento **FriendlyName** , que especifica un nombre descriptivo para el usuario.

        El elemento **User** podría contener un elemento **LineList** . Si un elemento **LineList** no está presente, se quitan todos los dispositivos de línea de este usuario.
    -   Elemento **LineList**

        El elemento **LineList** contiene información acerca de cada línea o dispositivo que podría estar asignado al usuario. Cada elemento **LineList** puede contener más de un elemento de **línea** .
    -   Elemento **line**

        Cada elemento de **línea** especifica un dispositivo de línea. Debe identificar cada dispositivo de línea agregando un elemento de **Dirección** o un elemento **PermanentID** en el elemento de **línea** .

        Para cada elemento de **línea** , puede establecer el atributo **Remove** . Si establece este atributo, el usuario ya no tiene asignado ese dispositivo de línea. Si no se establece este atributo, el usuario obtiene acceso a ese dispositivo de línea. No se proporciona ningún error si el dispositivo de línea no está disponible para el usuario.

## <a name="examples"></a>Ejemplos
- En los siguientes segmentos de código XML de ejemplo se muestra el uso correcto de los elementos definidos anteriormente.  
  - El siguiente código quita todos los dispositivos de línea asignados a user1.  
    ```
    <UserList>
      <User NoMerge="1">
        <DomainUser>domain1\user1</DomainUser>
      </User>
    </UserList>
    ```  
  - El siguiente código quita todos los dispositivos de línea asignados a user1 antes de asignar una línea con la dirección 99999. User1 no tendrá ningún otro dispositivo de líneas asignado, independientemente de si se asignó previamente algún dispositivo de línea.  
    ```
    <UserList>
      <User NoMerge="1">
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
  - El código siguiente agrega un dispositivo de línea para user1 sin eliminar ningún dispositivo de línea asignado previamente.  
    ```
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
  - En el código siguiente se agrega la dirección de línea 99999 y se quita la dirección de línea 88888 del acceso de Usuario1.  
    ```
    <UserList>
      <User>
        <DomainUser>domain1\user1</DomainUser>
        <FriendlyName>User1</FriendlyName>
        <LineList>
          <Line>
            <Address>99999</Address>
          </Line>
          <Line Remove="1">
            <Address>88888</Address>
          </Line>
        </LineList>
      </User>
    </UserList>
    ```  
  - El código siguiente agrega el dispositivo permanente 1000 y quita la línea 88888 del acceso de Usuario1.  
    ```
    <UserList>
      <User>
        <DomainUser>domain1\user1</DomainUser>
        <FriendlyName>User1</FriendlyName>
        <LineList>
          <Line>
            <PermanentID>1000</PermanentID>
          </Line>
          <Line Remove="1">
            <Address>88888</Address>
          </Line>
        </LineList>
      </User>
    </UserList>
    ```

-   La siguiente salida de ejemplo aparece después de especificar la opción de línea de comandos **/d** para mostrar la configuración actual de TAPI. Para cada proveedor de telefonía, se enumeran los dispositivos de línea asociados, así como las direcciones y los usuarios asociados a cada dispositivo de línea.  
    ```
    NDIS Proxy TAPI Service Provider
            Line: "WAN Miniport (L2TP)"
                    Permanent ID: 12345678910

    NDIS Proxy TAPI Service Provider
            Line: "LPT1DOMAIN1\User1"
                    Permanent ID: 12345678910

    Microsoft H.323 Telephony Service Provider
            Line: "H323 Line"
                    Permanent ID: 123456
                    Addresses:
                            BLDG1-TAPI32

    ```

#### <a name="additional-references"></a>Referencias adicionales

[Clave de sintaxis de línea de comandos](command-line-syntax-key.md)

[Información general del shell de comandos](https://technet.microsoft.com/library/cc737438(v=ws.10).aspx)
