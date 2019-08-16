---
title: Usar expresiones regulares de NPS
description: En este tema se explica el uso de expresiones regulares para la coincidencia de patrones en NPS en Windows Server 2016. Puede usar esta sintaxis para especificar las condiciones de los atributos de directiva de red y los dominios RADIUS.
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: bc22d29c-678c-462d-88b3-1c737dceca75
ms.author: pashort
author: shortpatti
ms.openlocfilehash: fec546e0608c36f9b3d907e486a0a3a24e7d1728
ms.sourcegitcommit: e04565e4a1fb7aaed04addd2bc87cc6ec4c82e81
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/16/2019
ms.locfileid: "69529901"
---
# <a name="use-regular-expressions-in-nps"></a>Usar expresiones regulares de NPS

>Se aplica a: Windows Server (canal semianual), Windows Server 2016

En este tema se explica el uso de expresiones regulares para la coincidencia de patrones en NPS en Windows Server 2016. Puede usar esta sintaxis para especificar las condiciones de los atributos de directiva de red y los dominios RADIUS.

## <a name="pattern-matching-reference"></a>Referencia de coincidencia de patrones

Puede utilizar la tabla siguiente como origen de referencia al crear expresiones regulares con sintaxis de coincidencia de patrones.


|  Óptico  |                                                                                 Descripción                                                                                  |                                                                 Ejemplo                                                                 |
|-------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|-----------------------------------------------------------------------------------------------------------------------------------------|
|     `\`|                                                              Marca el carácter siguiente como un carácter que debe coincidir.                                                               |                      `/n/ matches the character "n". The sequence /\n/ matches a line feed or newline character.`                       |
|     `^`     |                                                                 Coincide con el principio de la entrada o de la línea.                                                                  |                                                                 &nbsp;                                                                  |
|     `$`     |                                                                    Coincide con el final de la entrada o de la línea.                                                                     |                                                                 &nbsp;                                                                  |
|     `*`     |                                                             Coincide con el carácter anterior cero o más veces.                                                              |                                                  `/zo*/ matches either "z" or "zoo."`                                                   |
|     `+`     |                                                              Coincide con el carácter anterior una o más veces.                                                              |                                                   `/zo+/ matches "zoo" but not "z."`                                                    |
|     `?`     |                                                              Coincide con el carácter anterior cero o una vez.                                                              |                                                 `/a?ve?/ matches the "ve" in "never."`                                                  |
|     `.`     |                                                           Coincide con cualquier carácter individual excepto con un carácter de nueva línea.                                                           |                                                                 &nbsp;                                                                  |
| `(pattern)` |                         Coincide con "pattern" y recuerda la coincidencia.<br />Para buscar coincidencias `(` con `)` los caracteres literales y ( `\(` paréntesis `\)`), use o.                         |                                                                 &nbsp;                                                                  |
|   `x | y `  |                                                                               Coincide con x o y.                                                          |
|   `{n} `    |                                                          Coincide exactamente n veces \(n es un entero\-\)no negativo.                                                           |               `/o{2}/ does not match the "o" in "Bob," but matches the first two instances of the letter o in "foooood."`               |
|   `{n,}`    |                                                          Coincide al menos n veces \(n es un entero\-\)no negativo.                                                          | `/o{2,}/ does not match the "o" in "Bob" but matches all of the instances of the letter o in "foooood." /o{1,}/ is equivalent to /o+/.` |
|   `{n,m}`   |                                                Coincide como mínimo n veces \(m y n son enteros\)no\-negativos.                                                |                               `/o{1,3}/ matches the first three instances of the letter o in "fooooood."`                               |
|   `[xyz]`   |                                                       Coincide con cualquiera de los caracteres \(delimitados de un juego\)de caracteres.                                                        |                                                  `/[abc]/ matches the "a" in "plain."`                                                  |
|  `[^xyz]`   |                                                  Coincide con cualquier carácter que no esté \(incluido en un juego\)de caracteres negativos.                                                  |                                                 `/[^abc]/ matches the "p" in "plain."`                                                  |
|    `\b`     |                                                              Coincide con un límite \(de palabra, por ejemplo\), un espacio.                                                               |                                              `/ea*r\b/ matches the "er" in "never early."`                                              |
|    `\B`     |                                                                         Coincide con un límite que no es de palabra.                                                                          |                                             `/ea*r\B/ matches the "ear" in "never early."`                                              |
|    `\d`     |                                                       Coincide con un carácter \(de dígito equivalente a dígitos de 0\)a 9.                                                        |                                                                 &nbsp;                                                                  |
|    `\D`     |                                                           Coincide con un carácter \(que no es un dígito equivalente a. `[^0-9]` \)                                                           |                                                                 &nbsp;                                                                  |
|    `\f`     |                                                                        Coincide con un carácter de avance de la forma.                                                                        |                                                                 &nbsp;                                                                  |
|    `\n`     |                                                                        Coincide con un carácter de avance de línea.                                                                        |                                                                 &nbsp;                                                                  |
|    `\r`     |                                                                     Coincide con un carácter de retorno de carro.                                                                     |                                                                 &nbsp;                                                                  |
|    `\s`     |                                   Coincide con cualquier carácter de espacio en blanco incluido el espacio, la \(tabulación `[ \f\n\r\t\v]`y la fuente de formularios equivalentes a \).                                   |                                                                 &nbsp;                                                                  |
|    `\S`     |                                                  Coincide con cualquier carácter \(que no sea un espacio en blanco equivalente a. `[^ \f\n\r\t\v]` \)                                                   |                                                                 &nbsp;                                                                  |
|    `\t`     |                                                                           Coincide con un carácter de tabulación.                                                                           |                                                                 &nbsp;                                                                  |
|    `\v`     |                                                                      Coincide con un carácter de tabulación vertical.                                                                       |                                                                 &nbsp;                                                                  |
|    `\w`     |                                              Coincide con cualquier carácter de palabra, incluido \(el carácter `[A-Za-z0-9_]`de subrayado equivalente a \).                                              |                                                                 &nbsp;                                                                  |
|    `\W`     |                                           Coincide con cualquier\-carácter que no sea de palabra \(, excepto `[^A-Za-z0-9_]`el carácter de subrayado equivalente a \).                                           |                                                                 &nbsp;                                                                  |
|   `\num`    | Hace referencia a las \(coincidencias `?num`recordadas, donde NUM\)es un entero positivo.  Esta opción solo se puede usar en el cuadro de texto **reemplazar** al configurar la manipulación de atributos. |                                       `\1`reemplaza lo que se almacena en la primera coincidencia memorizada.                                       |
|   `/n/ `    |                      Permite la inserción de códigos ASCII en expresiones \( `?n`regulares, donde n es un valor\)de escape octal, hexadecimal o decimal.                       |                                                                 &nbsp;                                                                  |

## <a name="examples-for-network-policy-attributes"></a>Ejemplos de atributos de directiva de red

En los siguientes ejemplos se describe el uso de la sintaxis de coincidencia de patrones para especificar atributos de directiva de red:

- Para especificar todos los números de teléfono del código de área 899, la sintaxis es:

     `899.*`

- Para especificar un intervalo de direcciones IP que empiece por 192.168.1, la sintaxis es:

    `192\.168\.1\..+`

## <a name="examples-for-manipulation-of-the-realm-name-in-the-user-name-attribute"></a>Ejemplos de manipulación del nombre de dominio Kerberos en el atributo de nombre de usuario

En los siguientes ejemplos se describe el uso de la sintaxis de coincidencia de patrones para manipular nombres de dominio Kerberos para el atributo de nombre de usuario, que se encuentra en la pestaña **atributo** de las propiedades de una directiva de solicitud de conexión.

**Para quitar la parte del dominio Kerberos del atributo de nombre de usuario**

En un escenario de acceso telefónico de origen en el que un proveedor \(\) de servicios Internet envía solicitudes de conexión a un NPS de la organización, el proxy RADIUS del ISP podría requerir un nombre de dominio Kerberos para enrutar la solicitud de autenticación. Sin embargo, es posible que el NPS no reconozca la parte del nombre de dominio Kerberos del nombre de usuario. Por lo tanto, el proxy RADIUS del ISP debe quitar el nombre de dominio Kerberos antes de reenviarlo al NPS de la organización.

- Buscar: @microsoft \.com

- Reemplazar:

**Para reemplazar <em>user@example.microsoft.com</em> por _example. Microsoft. com\user_**

- Localización`(.*)@(.*)`

- Reemplace`$2\$1`



**Para reemplazar _dominio\usuario_ por _specific_domain\user_**

- Localización`(.*)\\(.*)`

- Reemplazar: *specific_domain*`\$2`



<strong>Para reemplazar el *usuario* por *user@specific_domain</strong>*

- Localización`$`

- Reemplazar: @*specific_domain*

## <a name="example-for-radius-message-forwarding-by-a-proxy-server"></a>Ejemplo de reenvío de mensajes RADIUS por un servidor proxy

Puede crear reglas de enrutamiento que reenvíen mensajes RADIUS con un nombre de dominio Kerberos especificado a un conjunto de servidores RADIUS cuando NPS se usa como proxy RADIUS. La siguiente es una sintaxis recomendada para el enrutamiento de solicitudes en función del nombre de dominio Kerberos.

- **Nombre NetBIOS**:`WCOAST`
- **Patrón**:`^wcoast\\`

En el ejemplo siguiente, wcoast.microsoft.com es un sufijo de nombre principal de usuario (UPN) único para DNS o Active Directory dominio wcoast.microsoft.com. Con el patrón proporcionado, el proxy NPS puede enrutar los mensajes según el nombre NetBIOS o el sufijo UPN del dominio.

- **Nombre NetBIOS**:`WCOAST`
- **Sufijo UPN**:`wcoast.microsoft.com`
- **Patrón**:`^wcoast\\|@wcoast\.microsoft\.com$`


Para obtener más información sobre la administración de NPS, consulte [administrar el servidor de directivas de redes](nps-manage-top.md).

Para obtener más información acerca de NPS, consulte [servidor de directivas de redes (NPS)](nps-top.md).
