---
title: tsecimp
description: Windows Commands topic for tsecimp, que importa la información de asignación de un archivo lenguaje de marcado extensible (XML) en el archivo de seguridad del servidor TAPI (TSEC. ini).
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: d7488ec6-0eff-45ff-89ee-9cbe752416bf
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 30a097bcd25e981f72a421b81b80b595343404ba
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/08/2020
ms.locfileid: "80832508"
---
# <a name="tsecimp"></a>tsecimp

Importa la información de asignación de un archivo lenguaje de marcado extensible (XML) en el archivo de seguridad del servidor TAPI (TSEC. ini). También puede usar este comando para mostrar la lista de proveedores TAPI y los dispositivos de línea asociados a cada uno de ellos, validar la estructura del archivo XML sin importar el contenido y comprobar la pertenencia al dominio.

## <a name="syntax"></a>Sintaxis

```
tsecimp /f <Filename> [{/v | /u}]
tsecimp /d
```

#### <a name="parameters"></a>Parámetros

|Parámetro|Descripción|
|---------|-----------|
|/f \<nombre de archivo >|Obligatorio. Especifica el nombre del archivo XML que contiene la información de asignaciones que desea importar.|
|/v|Valida la estructura del archivo XML sin importar la información al archivo Tsec.ini.|
|/u|Comprueba si cada usuario pertenece al dominio especificado en el archivo XML. El equipo donde use este parámetro deberá estar conectado a la red. Es posible que el parámetro reduzca considerablemente el rendimiento si se procesa una gran cantidad de información de asignaciones de usuarios.|
|/d|Muestra una lista de los proveedores de telefonía instalados. Para cada proveedor de telefonía, se enumeran los dispositivos de línea asociados, así como las direcciones y los usuarios asociados a cada dispositivo de línea.|
|/?|Muestra la Ayuda en el símbolo del sistema.|

## <a name="remarks"></a>Comentarios

-   El archivo XML desde el cual desea importar la información de asignaciones debe respetar la estructura que se describe a continuación.  
    -   Elemento **userList**

        **UserList** es el elemento superior del archivo XML.
    -   Elemento **User**

        Cada elemento de **usuario** contiene información acerca de un usuario que es miembro de un dominio. Es posible asignar uno o varios dispositivos de línea a cada usuario.

        Además, cada elemento de **usuario** podría tener un atributo denominado **NoMerge**. Cuando se especifica este atributo, se quitan todas las asignaciones de dispositivos de línea del usuario antes de realizar nuevas asignaciones. Puede usar este atributo para quitar fácilmente las asignaciones de usuario no deseadas. De forma predeterminada, este atributo no está establecido.

        El elemento **User** debe contener un único elemento **DomainUserName** , que especifica el dominio y el nombre de usuario del usuario. El elemento **User** también puede contener un elemento **FriendlyName** , que especifica un nombre descriptivo para el usuario.

        El elemento **User** podría contener un elemento **LineList** . Si un elemento **LineList** no está presente, se quitan todos los dispositivos de línea de este usuario.
    -   Elemento **LineList**

        El elemento **LineList** contiene información acerca de cada línea o dispositivo que podría estar asignado al usuario. Cada elemento **LineList** puede contener más de un elemento de **línea** .
    -   Elemento **line**

        Cada elemento de **línea** especifica un dispositivo de línea. Debe identificar cada dispositivo de línea agregando un elemento de **Dirección** o un elemento **PermanentID** en el elemento de **línea** .

        Para cada elemento de **línea** , puede establecer el atributo **Remove** . Si se establece este atributo, el dispositivo de línea deja de estar asignado al usuario. Si no se establece este atributo, el usuario obtiene acceso a dicho dispositivo de línea. No se proporciona ningún error si el dispositivo de línea no está disponible para el usuario.

## <a name="examples"></a>Ejemplos
- Los siguientes segmentos de código XML de ejemplo muestran el uso correcto de los elementos definidos anteriormente.  
  - El siguiente código quita todos los dispositivos de línea asignados a user1.  
    ```
    <UserList>
      <User NoMerge=1>
        <DomainUser>domain1\user1</DomainUser>
      </User>
    </UserList>
    ```  
  - El siguiente código quita todos los dispositivos de línea asignados a user1 antes de asignar una línea con la dirección 99999. El usuario1 no tendrá asignados otros dispositivos de línea, independientemente de que haya tenido dispositivos de línea asignados anteriormente.  
    ```
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
  - El siguiente código agrega un dispositivo de línea para usuario1 sin eliminar los dispositivos de línea asignados anteriormente.  
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
          <Line Remove=1>
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
          <Line Remove=1>
            <Address>88888</Address>
          </Line>
        </LineList>
      </User>
    </UserList>
    ```

-   La siguiente salida de ejemplo aparece después de especificar la opción de línea de comandos **/d** para mostrar la configuración actual de TAPI. Para cada proveedor de telefonía, se enumeran los dispositivos de línea asociados, así como las direcciones y los usuarios asociados a cada dispositivo de línea.  
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

## <a name="additional-references"></a>Referencias adicionales

- [Clave de sintaxis de línea de comandos](command-line-syntax-key.md)

[Información general del shell de comandos](https://technet.microsoft.com/library/cc737438(v=ws.10).aspx)
