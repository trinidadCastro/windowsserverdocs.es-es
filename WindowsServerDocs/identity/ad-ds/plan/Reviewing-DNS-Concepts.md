---
ms.assetid: 133474ee-316d-4b1c-acc6-ad5434a064d5
title: Revisar los conceptos de DNS
description: ''
ms.author: joflore
author: MicrosoftGuyJFlo
manager: mtillman
ms.date: 08/08/2018
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: d077ecc30caca9b8f3b382af624121fec729afae
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59890506"
---
# <a name="reviewing-dns-concepts"></a>Revisar los conceptos de DNS

>Se aplica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Sistema de nombres de dominio (DNS) es una base de datos distribuida que representa un espacio de nombres. El espacio de nombres contiene toda la información necesaria para que cualquier cliente buscar cualquier nombre. Cualquier servidor DNS puede responder a consultas sobre cualquier nombre dentro de su espacio de nombres. Un servidor DNS responde a las consultas en una de las maneras siguientes:  
  
- Si la respuesta está en su memoria caché, responde a la consulta de la memoria caché.  
- Si la respuesta está en una zona hospedada por el servidor DNS, responde a la consulta de su zona. Una zona es una parte del árbol DNS almacenado en un servidor DNS. Cuando un servidor DNS hospeda una zona, es autoritativo para los nombres de esa zona (es decir, el servidor DNS puede responder a consultas de nombres en la zona). Por ejemplo, un servidor que hospeda la zona contoso.com puede responder a las consultas de nombres en contoso.com.  
- Si el servidor no puede responder a la consulta desde su caché o zonas, consulta otros servidores para la respuesta.  

Es importante comprender las características principales de DNS, como la delegación, la resolución recursiva de nombres y las zonas DNS integradas en Active Directory, ya que tienen un impacto directo en el diseño de la estructura lógica de Active Directory.  
  
Para obtener más información acerca de DNS y Active Directory Domain Services (AD DS), consulte [DNS y AD DS](../../ad-ds/plan/DNS-and-AD-DS.md).  
  
## <a name="delegation"></a>Delegación

Para que un servidor DNS responder a consultas sobre cualquier nombre, debe tener una ruta de acceso directo o indirecto a cada zona en el espacio de nombres. Estas rutas de acceso se crean por medio de delegación. Una delegación es un registro en una zona primaria que enumera un servidor de nombres que sea autoritativo para la zona en el siguiente nivel de la jerarquía. Las delegaciones hacen posible que los servidores en una zona para hacer referencia a los clientes a servidores en otras zonas. La siguiente ilustración muestra un ejemplo de delegación.  
  
![Conceptos de DNS](../../media/Reviewing-DNS-Concepts/0c24b576-d41a-4e5d-ad3d-6be81e095835.gif)  
  
El servidor de raíz DNS hospeda la zona raíz que se representa como un punto (. ). La zona raíz contiene una delegación a una zona en el siguiente nivel de la jerarquía, la zona com. La delegación en la zona raíz indica al servidor de raíz DNS que, para buscar la zona de com, debe ponerse en contacto con el servidor Com. Del mismo modo, la delegación en la zona com indica al servidor Com, para buscar la zona contoso.com, debe ponerse en contacto con el servidor de Contoso.  
  
> [!NOTE]  
> Una delegación usa dos tipos de registros. El registro de recursos de nombres (NS) de servidor proporciona el nombre de un servidor autoritativo. Host (A) y registros de recursos (AAAA) de host proporcionan IP versión 4 (IPv4) y IP versión 6 (IPv6) las direcciones de un servidor autoritativo.  
  
Este sistema de las delegaciones y zonas crea un árbol jerárquico que representa el espacio de nombres DNS. Cada zona representa una capa en la jerarquía, y cada delegación representa una rama del árbol.  
  
Mediante el uso de la jerarquía de las delegaciones y zonas, un servidor de raíz DNS puede encontrar cualquier nombre en el espacio de nombres DNS. La zona raíz incluye las delegaciones que conducen directa o indirectamente a todas las demás zonas en la jerarquía. Cualquier servidor que puede consultar el servidor de raíz DNS puede usar la información de las delegaciones para buscar cualquier nombre en el espacio de nombres.  
  
## <a name="recursive-name-resolution"></a>Resolución de nombres recursivos

Resolución recursiva de nombres es el proceso por el que un servidor DNS usa la jerarquía de las delegaciones y zonas para responder a las consultas para los que no es autoritativo.  
  
En algunas configuraciones, los servidores DNS incluyen sugerencias de raíz (es decir, una lista de nombres y direcciones IP) que permiten a los servidores raíz DNS de la consulta. En otras configuraciones, los servidores reenviar todas las consultas que no responden a otro servidor. Las sugerencias de raíz y reenvío son ambos métodos que pueden usar servidores DNS para resolver las consultas para los que no están autorizados.  
  
### <a name="resolving-names-by-using-root-hints"></a>Resolver nombres mediante sugerencias de raíz

Las sugerencias de raíz permiten a cualquier servidor DNS ubicar los servidores raíz DNS. Después de un servidor DNS encuentra el servidor raíz DNS, puede resolver cualquier consulta para ese espacio de nombres. La ilustración siguiente describe cómo DNS resuelve un nombre mediante el uso de sugerencias de raíz.  
  
![Conceptos de DNS](../../media/Reviewing-DNS-Concepts/1c044845-b104-4262-a7af-474ba3558a85.gif)  
  
En este ejemplo, se producen los eventos siguientes:  
  
1. Un cliente envía una consulta recursiva en un servidor DNS para solicitar la dirección IP que se corresponde con el nombre ftp.contoso.com. Una consulta recursiva indica que el cliente desea recibir una respuesta definitiva a su consulta. La respuesta a la consulta recursiva debe ser una dirección válida o un mensaje que indica que no se encuentra la dirección.  
2. Dado que el servidor DNS no es autoritativo para el nombre y no tiene en su caché la respuesta, el servidor DNS usa las sugerencias de raíz para buscar la dirección IP del servidor DNS raíz.  
3. El servidor DNS usa una consulta iterativa para pedir al servidor de raíz DNS para resolver el nombre ftp.contoso.com. Una consulta iterativa indica que el servidor acepta una referencia a otro servidor en lugar de una respuesta definitiva a la consulta. Dado que el ftp.contoso.com nombre termina con la etiqueta de com, el servidor de raíz DNS devuelve una referencia al servidor Com que hospeda la zona com.  
4. El servidor DNS usa una consulta iterativa para pedir al servidor Com para resolver el nombre ftp.contoso.com. Dado que el ftp.contoso.com nombre termina con el nombre de "contoso.com", el servidor Com devuelve una referencia a Contoso servidor que hospeda la zona contoso.com.  
5. El servidor DNS usa una consulta iterativa para pedir al servidor de Contoso para resolver el nombre ftp.contoso.com. El servidor Contoso busca la respuesta de sus datos de zona y, a continuación, devuelve la respuesta al servidor.  
6. El servidor, a continuación, devuelve el resultado al cliente.  
  
### <a name="resolving-names-by-using-forwarding"></a>Resolución de nombres mediante el reenvío

Reenvío permite a la resolución de nombres de ruta a través de servidores específicos en lugar de usar las sugerencias de raíz. La ilustración siguiente describe cómo DNS resuelve un nombre mediante el reenvío.  
  
![Conceptos de DNS](../../media/Reviewing-DNS-Concepts/05bc2eb0-1033-4e53-ae30-244fa247d000.gif)  
  
En este ejemplo, se producen los eventos siguientes:  
  
1. Un cliente consulta un servidor DNS para el nombre de ftp.contoso.com.  
2. El servidor DNS reenvía la consulta a otro servidor DNS, conocido como reenviador.  
3. Dado que el reenviador no es autoritativo para el nombre y no tiene en su caché la respuesta, usa las sugerencias de raíz para buscar la dirección IP del servidor DNS raíz.  
4. El reenviador utiliza una consulta iterativa para pedir al servidor de raíz DNS para resolver el nombre ftp.contoso.com. Dado que el ftp.contoso.com nombre termina con el nombre de com, el servidor de raíz DNS devuelve una referencia al servidor Com que hospeda la zona com.  
5. El reenviador utiliza una consulta iterativa para pedir al servidor Com para resolver el nombre ftp.contoso.com. Dado que el ftp.contoso.com nombre termina con el nombre de "contoso.com", el servidor Com devuelve una referencia a Contoso servidor que hospeda la zona contoso.com.  
6. El reenviador utiliza una consulta iterativa para pedir al servidor de Contoso para resolver el nombre ftp.contoso.com. El servidor Contoso busca la respuesta de sus archivos de zona y, a continuación, devuelve la respuesta al servidor.  
7. El reenviador, a continuación, devuelve el resultado al servidor DNS original.  
8. El servidor DNS original, a continuación, devuelve el resultado al cliente.  
