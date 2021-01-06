---
title: Usar expresiones regulares de NPS
description: En este tema se explica el uso de expresiones regulares para la coincidencia de patrones en NPS en Windows Server. Puede usar esta sintaxis para especificar las condiciones de los atributos de directiva de red y dominios kerberos de RADIUS.
manager: brianlic
ms.topic: article
ms.assetid: bc22d29c-678c-462d-88b3-1c737dceca75
ms.author: jgerend
author: jasongerend
ms.date: 08/16/2019
ms.openlocfilehash: b1c1acd5feb39304a7c507aa88ea15156fb5e8dd
ms.sourcegitcommit: 40905b1f9d68f1b7d821e05cab2d35e9b425e38d
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/06/2021
ms.locfileid: "97947101"
---
# <a name="use-regular-expressions-in-nps"></a>Usar expresiones regulares de NPS

> Se aplica a: Windows Server 2019, Windows Server 2016, Windows Server (canal semianual)

En este tema se explica el uso de expresiones regulares para la coincidencia de patrones en NPS en Windows Server. Puede usar esta sintaxis para especificar las condiciones de los atributos de directiva de red y dominios kerberos de RADIUS.

## <a name="pattern-matching-reference"></a>Referencia para la coincidencia de patrón

Use la tabla siguiente como referencia cuando vaya a crear expresiones regulares con sintaxis de coincidencia de patrón. Tenga en cuenta que los patrones de expresiones regulares suelen estar rodeados por barras diagonales (/).

|  Carácter  |  Descripción  |   Ejemplo                                                                 |
| ----------- | ------------- | ------------------------------------------------------------------------  |
|     `\ `     | Indica que el carácter que sigue es un carácter especial o que se debe interpretar literalmente.  | `/n/ matches the character "n" while the sequence /\n/ matches a line feed or newline character.`  |
|     `^`     |                                                                 Coincide con el principio de entrada o línea.                                                                  |                                                                 &nbsp;                                                                  |
|     `$`     |                                                                    Coincide con el final de entrada o línea.                                                                     |                                                                 &nbsp;                                                                  |
|     `*`     |                                                             Coincide con el carácter anterior ninguna o más veces.                                                              |                                                  `/zo*/ matches either "z" or "zoo."`                                                   |
|     `+`     |                                                              Coincide con el carácter anterior una o más veces.                                                              |                                                   `/zo+/ matches "zoo" but not "z."`                                                    |
|     `?`     |                                                              Coincide con el carácter anterior una vez o ninguna.                                                              |                                                 `/a?ve?/ matches the "ve" in "never."`                                                  |
|     `.`     |                                                           Coincide con cualquier carácter sencillo, excepto con un carácter de línea nueva.                                                           |                                                                 &nbsp;                                                                  |
| `(pattern)` |                         Coincide con "pattern" y recuerda la coincidencia.<br />Para buscar coincidencias con los caracteres literales `(` y `)` (paréntesis), use `\(` o `\)` .                         |                                                                 &nbsp;                                                                  |
|   `x | y `  |                                                                               Coincide con x o y.                                                          |
|   `{n} `    |                                                          Coincide exactamente n veces \( n es un \- entero no negativo \) .                                                           |               `/o{2}/ does not match the "o" in "Bob," but matches the first two instances of the letter o in "foooood."`               |
|   `{n,}`    |                                                          Coincide al menos n veces \( n es un \- entero no negativo \) .                                                          | `/o{2,}/ does not match the "o" in "Bob" but matches all of the instances of the letter o in "foooood." /o{1,}/ is equivalent to /o+/.` |
|   `{n,m}`   |                                                Coincide como mínimo n veces \( m y n son \- enteros no negativos \) .                                                |                               `/o{1,3}/ matches the first three instances of the letter o in "fooooood."`                               |
|   `[xyz]`   |                                                       Coincide con cualquiera de los caracteres delimitados de \( un juego de caracteres \) .                                                        |                                                  `/[abc]/ matches the "a" in "plain."`                                                  |
|  `[^xyz]`   |                                                  Coincide con cualquier carácter que no esté incluido en \( un juego de caracteres negativos \) .                                                  |                                                 `/[^abc]/ matches the "p" in "plain."`                                                  |
|    `\b`     |                                                              Coincide con un límite \( de palabra, por ejemplo, un espacio \) .                                                               |                                              `/ea*r\b/ matches the "er" in "never early."`                                              |
|    `\B`     |                                                                         Coincide si no es un límite de palabra.                                                                          |                                             `/ea*r\B/ matches the "ear" in "never early."`                                              |
|    `\d`     |                                                       Coincide con un carácter \( de dígito equivalente a dígitos de 0 a 9 \) .                                                        |                                                                 &nbsp;                                                                  |
|    `\D`     |                                                           Coincide con un carácter que no es un dígito \( equivalente a `[^0-9]` \) .                                                           |                                                                 &nbsp;                                                                  |
|    `\f`     |                                                                        Coincide con un carácter de avance de página.                                                                        |                                                                 &nbsp;                                                                  |
|    `\n`     |                                                                        Coincide con un carácter de salto de línea.                                                                        |                                                                 &nbsp;                                                                  |
|    `\r`     |                                                                     Coincide con un carácter de retorno de carro.                                                                     |                                                                 &nbsp;                                                                  |
|    `\s`     |                                   Coincide con cualquier carácter de espacio en blanco incluido el espacio, la tabulación y la fuente de formularios \( equivalentes a `[ \f\n\r\t\v]` \) .                                   |                                                                 &nbsp;                                                                  |
|    `\S`     |                                                  Coincide con cualquier carácter que no sea un espacio en blanco \( equivalente a `[^ \f\n\r\t\v]` \) .                                                   |                                                                 &nbsp;                                                                  |
|    `\t`     |                                                                           Coincide con un carácter de tabulación.                                                                           |                                                                 &nbsp;                                                                  |
|    `\v`     |                                                                      Coincide con un carácter de tabulación vertical.                                                                       |                                                                 &nbsp;                                                                  |
|    `\w`     |                                              Coincide con cualquier carácter de palabra, incluido el carácter de subrayado \( equivalente a `[A-Za-z0-9_]` \) .                                              |                                                                 &nbsp;                                                                  |
|    `\W`     |                                           Coincide con cualquier carácter que no sea \- de palabra, excepto el carácter de subrayado \( equivalente a `[^A-Za-z0-9_]` \) .                                           |                                                                 &nbsp;                                                                  |
|   `\num`    | Hace referencia a las coincidencias recordadas \( `?num` , donde NUM es un entero positivo \) .  Esta opción solo se puede usar en el cuadro de texto **reemplazar** al configurar la manipulación de atributos. |                                       `\1` reemplaza lo que se almacena en la primera coincidencia memorizada.                                       |
|   `/n/ `    |                      Permite la inserción de códigos ASCII en expresiones regulares \( `?n` , donde n es un valor de escape octal, hexadecimal o decimal \) .                       |                                                                 &nbsp;                                                                  |

## <a name="examples-for-network-policy-attributes"></a>Ejemplos de atributos de directiva de red

Los ejemplos siguientes describen el uso de la sintaxis de coincidencia de patrón para especificar atributos de directiva de red:

- Para especificar todos los números de teléfono del código de área 899, la sintaxis es:

     `899.*`

- Para especificar un intervalo de direcciones IP que empiece por 192.168.1, la sintaxis es:

    `192\.168\.1\..+`

## <a name="examples-for-manipulation-of-the-realm-name-in-the-user-name-attribute"></a>Ejemplos de tratamiento del nombre de territorio en el atributo User Name

En los siguientes ejemplos se describe el uso de la sintaxis de coincidencia de patrones para manipular nombres de dominio Kerberos para el atributo de nombre de usuario, que se encuentra en la pestaña **atributo** de las propiedades de una directiva de solicitud de conexión.

**Para quitar la parte de dominio kerberos del atributo User Name**

En un escenario de acceso telefónico de origen en el que un proveedor de servicios \( Internet \) envía solicitudes de conexión a un NPS de la organización, el proxy RADIUS del ISP podría requerir un nombre de dominio Kerberos para enrutar la solicitud de autenticación. Sin embargo, es posible que el NPS no reconozca la parte del nombre de dominio Kerberos del nombre de usuario. Por lo tanto, el proxy RADIUS del ISP debe quitar el nombre de dominio Kerberos antes de reenviarlo al NPS de la organización.

- Buscar: @microsoft \. com

- Sustituya:

**Para reemplazar <em>user@example.microsoft.com</em> por _example. Microsoft. com\user_**

- Localización`(.*)@(.*)`

- Reemplace`$2\$1`



**Para reemplazar _dominio\usuario_ por _specific_domain \ \_**

- Localización`(.*)\\(.*)`

- Reemplazar: *specific_domain*`\$2`



<strong>Para reemplazar el *usuario* por *user@specific_domain</strong>*

- Localización`$`

- Reemplazar: @*specific_domain*

## <a name="example-for-radius-message-forwarding-by-a-proxy-server"></a>Ejemplo de reenvío de mensajes RADIUS por parte de un servidor proxy

Puede crear reglas de enrutamiento que reenvíen mensajes RADIUS con un nombre de territorio específico a un conjunto determinado de servidores RADIUS cuando NPS se use como un proxy RADIUS. A continuación, se indica la sintaxis recomendada para el enrutamiento de solicitudes en función del nombre de territorio.

- **Nombre NetBIOS**: `WCOAST`
- **Patrón**:      `^wcoast\\`

En el ejemplo siguiente, wcoast.microsoft.com es un sufijo de nombre principal de usuario (UPN) único para DNS o Active Directory dominio wcoast.microsoft.com. Con el patrón proporcionado, el proxy NPS puede enrutar los mensajes según el nombre NetBIOS o el sufijo UPN del dominio.

- **Nombre NetBIOS**: `WCOAST`
- **Sufijo UPN**:   `wcoast.microsoft.com`
- **Patrón**:      `^wcoast\\|@wcoast\.microsoft\.com$`


Para obtener más información sobre la administración de NPS, consulte [administrar el servidor de directivas de redes](nps-manage-top.md).

Para obtener más información acerca de NPS, consulte [servidor de directivas de redes (NPS)](nps-top.md).
