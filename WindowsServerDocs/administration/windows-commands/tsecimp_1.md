---
title: tsecimp
description: 'Tema de los comandos de Windows para ***- '
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
ms.openlocfilehash: a5ed2ef8b1d0238a3608dabdd165a255855a304d
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/31/2019
ms.locfileid: "66440873"
---
# <a name="tsecimp"></a>tsecimp



Importa información de asignación de un archivo de lenguaje de marcado Extensible (XML) en el archivo de seguridad de servidor TAPI (Tsec.ini). También puede usar este comando para mostrar la lista de proveedores TAPI y los dispositivos de línea asociados a cada uno de ellos, validar la estructura del archivo XML sin importar el contenido y comprobar la pertenencia a dominio.

## <a name="syntax"></a>Sintaxis

```
tsecimp /f <Filename> [{/v | /u}]
tsecimp /d
```

### <a name="parameters"></a>Parámetros

|Parámetro|Descripción|
|---------|-----------|
|/f \<nombreDeArchivo >|Obligatorio. Especifica el nombre del archivo XML que contiene la información de asignación que desea importar.|
|/v|Valida la estructura del archivo XML sin importar la información al archivo Tsec.ini.|
|/u|Comprueba si cada usuario es miembro del dominio especificado en el archivo XML. El equipo en el que se usa este parámetro debe estar conectado a la red. Este parámetro podría Reduzca considerablemente el rendimiento si se procesan una gran cantidad de información de asignación de usuario.|
|/d|Muestra una lista de proveedores de telefonía instalados. Para cada proveedor de telefonía, se enumeran los dispositivos de línea asociados, así como las direcciones y los usuarios asociados con cada dispositivo de línea.|
|/?|Muestra la ayuda en el símbolo del sistema.|

## <a name="remarks"></a>Comentarios

-   El archivo XML desde el que desea importar información de asignación debe seguir la estructura descrita a continuación.  
    -   **UserList** elemento

        El **UserList** es el elemento superior del archivo XML.
    -   **Usuario** elemento

        Cada **usuario** elemento contiene información sobre un usuario que sea miembro de un dominio. Cada usuario puede asignarse a uno o varios dispositivos de línea.

        Además, cada **usuario** elemento podría tener un atributo denominado **NoMerge**. Cuando se especifica este atributo, se quitan todas las asignaciones de dispositivo de línea actual del usuario antes de realizar nuevas. Puede usar este atributo para quitar fácilmente las asignaciones de usuario no deseado. De forma predeterminada, no se establece este atributo.

        El **usuario** elemento debe contener un único **DomainUserName** elemento, que especifica el nombre de usuario y dominio del usuario. El **usuario** elemento también puede contener un **FriendlyName** elemento, que especifica un nombre descriptivo para el usuario.

        El **usuario** elemento podría contener un **LineList** elemento. Si un **LineList** elemento no está presente, se quitan todos los dispositivos de línea para este usuario.
    -   **LineList** elemento

        El **LineList** elemento contiene información acerca de cada línea o dispositivo que puedan estar asignado al usuario. Cada **LineList** elemento puede contener más de un **línea** elemento.
    -   **Línea** elemento

        Cada **línea** elemento especifica un dispositivo de línea. Debe identificar cada dispositivo de línea mediante la inclusión un **dirección** elemento o un **PermanentID** elemento bajo el **línea** elemento.

        Para cada **línea** elemento, puede establecer el **quitar** atributo. Si establece este atributo, el usuario ya no está asignado ese dispositivo de línea. Si no se establece este atributo, el usuario obtiene acceso al dispositivo de línea. No se proporciona ningún error si el dispositivo de línea no está disponible para el usuario.

## <a name="examples"></a>Ejemplos
- Los siguientes segmentos de código XML de ejemplo muestran el uso correcto de los elementos definidos anteriormente.  
  - El código siguiente quita todos los dispositivos de línea asignados a usuario1.  
    ```
    <UserList>
      <User NoMerge="1">
        <DomainUser>domain1\user1</DomainUser>
      </User>
    </UserList>
    ```  
  - El código siguiente quita todos los dispositivos de línea asignados a Usuario1 antes de asignar una línea con la dirección 99999. Este User1 no tendría ningún otros dispositivos de línea asignados, con independencia de si los dispositivos de línea se asignaron previamente.  
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
  - El código siguiente agrega un dispositivo de línea para usuario1 sin eliminar los dispositivos de línea asignados anteriormente.  
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
  - El código siguiente agrega la dirección de línea 99999 y quita la dirección de línea 88888 del acceso de User1.  
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
  - El código siguiente agrega el dispositivo permanente 1000 y quita la línea 88888 del acceso de User1.  
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


~~~
    ```  
~~~
-   The following sample output appears after the **/d** command-line option is specified to display the current TAPI configuration. For each telephony provider, the associated line devices are listed, as well as the addresses and users associated with each line device.  
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

#### Additional references

[Command-Line Syntax Key](command-line-syntax-key.md)

[Command shell overview](https://technet.microsoft.com/library/cc737438(v=ws.10).aspx)