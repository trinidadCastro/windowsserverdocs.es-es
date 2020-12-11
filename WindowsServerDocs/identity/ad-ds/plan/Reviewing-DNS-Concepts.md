---
description: Más información acerca de cómo revisar los conceptos de DNS
ms.assetid: 133474ee-316d-4b1c-acc6-ad5434a064d5
title: Revisar los conceptos de DNS
ms.author: daveba
author: iainfoulds
manager: daveba
ms.date: 08/08/2018
ms.topic: article
ms.openlocfilehash: 331d62ab65e8030057824b82b23ced3ede7e035f
ms.sourcegitcommit: 65b6de6b44d41f1180c45db11cdd60cb2a093b46
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/10/2020
ms.locfileid: "97042603"
---
# <a name="reviewing-dns-concepts"></a>Revisar los conceptos de DNS

>Se aplica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

El sistema de nombres de dominio (DNS) es una base de datos distribuida que representa un espacio de nombres. El espacio de nombres contiene toda la información necesaria para que cualquier cliente busque cualquier nombre. Cualquier servidor DNS puede responder a las consultas sobre cualquier nombre dentro de su espacio de nombres. Un servidor DNS responde a las consultas de una de las siguientes maneras:

- Si la respuesta está en su caché, responde a la consulta de la memoria caché.
- Si la respuesta está en una zona hospedada por el servidor DNS, responde a la consulta de su zona. Una zona es una parte del árbol DNS almacenada en un servidor DNS. Cuando un servidor DNS hospeda una zona, es autoritativo para los nombres de esa zona (es decir, el servidor DNS puede responder a las consultas de cualquier nombre de la zona). Por ejemplo, un servidor que hospede la zona contoso.com puede responder a las consultas de cualquier nombre de contoso.com.
- Si el servidor no puede responder a la consulta desde su memoria caché o zonas, consulta a otros servidores la respuesta.

Es importante comprender las características principales de DNS, como la delegación, la resolución de nombres recursivo y las zonas DNS integradas en Active Directory, porque tienen un impacto directo en el diseño de la estructura lógica de Active Directory.

Para obtener más información acerca de DNS y Active Directory Domain Services (AD DS), consulte [DNS y AD DS](../../ad-ds/plan/DNS-and-AD-DS.md).

## <a name="delegation"></a>Delegación

Para que un servidor DNS responda a las consultas de cualquier nombre, debe tener una ruta de acceso directa o indirecta a cada zona del espacio de nombres. Estas rutas de acceso se crean mediante la delegación. Una delegación es un registro de una zona primaria que muestra un servidor de nombres que es autoritativo para la zona en el siguiente nivel de la jerarquía. Las delegaciones permiten que los servidores de una zona hagan referencia a los clientes a los servidores de otras zonas. En la ilustración siguiente se muestra un ejemplo de delegación.

![Conceptos de DNS](../../media/Reviewing-DNS-Concepts/0c24b576-d41a-4e5d-ad3d-6be81e095835.gif)

El servidor raíz DNS hospeda la zona raíz representada como un punto (. ). La zona raíz contiene una delegación a una zona en el siguiente nivel de la jerarquía, la zona com. La delegación de la zona raíz indica al servidor raíz de DNS que, para encontrar la zona com, debe ponerse en contacto con el servidor com. Del mismo modo, la delegación de la zona com indica al servidor com que, para encontrar la zona contoso.com, debe ponerse en contacto con el servidor de contoso.

> [!NOTE]
> Una delegación utiliza dos tipos de registros. El registro de recursos del servidor de nombres (NS) proporciona el nombre de un servidor autoritativo. Los registros de recursos de host (A) y host (AAAA) proporcionan direcciones IP versión 4 (IPv4) y versión 6 (IPv6) de un servidor autoritativo.

Este sistema de zonas y delegaciones crea un árbol jerárquico que representa el espacio de nombres DNS. Cada zona representa una capa en la jerarquía y cada delegación representa una rama del árbol.

Mediante el uso de la jerarquía de zonas y delegaciones, un servidor raíz DNS puede encontrar cualquier nombre en el espacio de nombres DNS. La zona raíz incluye delegaciones que conducen directa o indirectamente a todas las demás zonas de la jerarquía. Cualquier servidor que pueda consultar el servidor raíz DNS puede usar la información de las delegaciones para buscar cualquier nombre en el espacio de nombres.

## <a name="recursive-name-resolution"></a>Resolución de nombres recursivos

La resolución de nombres recursivo es el proceso por el que un servidor DNS usa la jerarquía de zonas y delegaciones para responder a las consultas para las que no es autoritativa.

En algunas configuraciones, los servidores DNS incluyen sugerencias de raíz (es decir, una lista de nombres y direcciones IP) que les permiten consultar los servidores raíz DNS. En otras configuraciones, los servidores reenvían todas las consultas que no pueden responder a otro servidor. El reenvío y las sugerencias de raíz son métodos que los servidores DNS pueden utilizar para resolver consultas para las que no son autoritativas.

### <a name="resolving-names-by-using-root-hints"></a>Resolver nombres mediante sugerencias de raíz

Las sugerencias de raíz permiten a cualquier servidor DNS localizar los servidores raíz DNS. Después de que un servidor DNS encuentre el servidor raíz DNS, puede resolver cualquier consulta para ese espacio de nombres. En la ilustración siguiente se describe el modo en que DNS resuelve un nombre mediante sugerencias de raíz.

![Conceptos de DNS](../../media/Reviewing-DNS-Concepts/1c044845-b104-4262-a7af-474ba3558a85.gif)

En este ejemplo, se producen los siguientes eventos:

1. Un cliente envía una consulta recursiva a un servidor DNS para solicitar la dirección IP que se corresponda con el nombre ftp.contoso.com. Una consulta recursiva indica que el cliente desea una respuesta definitiva a su consulta. La respuesta a la consulta recursiva debe ser una dirección válida o un mensaje que indique que no se puede encontrar la dirección.
2. Dado que el servidor DNS no es autoritativo para el nombre y no tiene la respuesta en su caché, el servidor DNS usa sugerencias de raíz para encontrar la dirección IP del servidor raíz DNS.
3. El servidor DNS usa una consulta iterativa para solicitar al servidor raíz DNS que resuelva el nombre ftp.contoso.com. Una consulta iterativa indica que el servidor aceptará una referencia a otro servidor en lugar de una respuesta definitiva a la consulta. Dado que el nombre ftp.contoso.com finaliza con la etiqueta com, el servidor raíz DNS devuelve una referencia al servidor com que hospeda la zona com.
4. El servidor DNS usa una consulta iterativa para pedir al servidor com que resuelva el nombre ftp.contoso.com. Dado que el nombre ftp.contoso.com termina con el nombre contoso.com, el servidor com devuelve una referencia al servidor de Contoso que hospeda la zona contoso.com.
5. El servidor DNS usa una consulta iterativa para solicitar al servidor de Contoso que resuelva el nombre ftp.contoso.com. El servidor de Contoso encuentra la respuesta en los datos de la zona y, a continuación, devuelve la respuesta al servidor.
6. A continuación, el servidor devuelve el resultado al cliente.

### <a name="resolving-names-by-using-forwarding"></a>Resolución de nombres mediante el reenvío

El reenvío le permite enrutar la resolución de nombres a través de servidores específicos en lugar de usar sugerencias de raíz. En la ilustración siguiente se describe el modo en que DNS resuelve un nombre mediante el reenvío.

![Conceptos de DNS](../../media/Reviewing-DNS-Concepts/05bc2eb0-1033-4e53-ae30-244fa247d000.gif)

En este ejemplo, se producen los siguientes eventos:

1. Un cliente consulta un servidor DNS para el nombre ftp.contoso.com.
2. El servidor DNS reenvía la consulta a otro servidor DNS, conocido como reenviador.
3. Dado que el reenviador no es autoritativo para el nombre y no tiene la respuesta en su caché, usa sugerencias de raíz para encontrar la dirección IP del servidor raíz DNS.
4. El reenviador utiliza una consulta iterativa para solicitar al servidor raíz DNS que resuelva el nombre ftp.contoso.com. Dado que el nombre ftp.contoso.com finaliza con el nombre com, el servidor raíz DNS devuelve una referencia al servidor com que hospeda la zona com.
5. El reenviador utiliza una consulta iterativa para solicitar al servidor com que resuelva el nombre ftp.contoso.com. Dado que el nombre ftp.contoso.com termina con el nombre contoso.com, el servidor com devuelve una referencia al servidor de Contoso que hospeda la zona contoso.com.
6. El reenviador utiliza una consulta iterativa para solicitar al servidor de Contoso que resuelva el nombre ftp.contoso.com. El servidor de Contoso encuentra la respuesta en sus archivos de zona y, a continuación, devuelve la respuesta al servidor.
7. A continuación, el reenviador devuelve el resultado al servidor DNS original.
8. A continuación, el servidor DNS original devuelve el resultado al cliente.
