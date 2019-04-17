---
ms.assetid: 133474ee-316d-4b1c-acc6-ad5434a064d5
title: Revisar los conceptos de DNS
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 29c4c3d1d785d1b3b1fa1926eca8298aab2b3ee3
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/03/2017
---
# <a name="reviewing-dns-concepts"></a>Revisar los conceptos de DNS

>Se aplica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Sistema de nombres de dominio (DNS) es una base de datos distribuida que representa un espacio de nombres. El espacio de nombres contiene toda la información necesaria para que cualquier cliente buscar el nombre. Cualquier servidor DNS puede responder a las consultas sobre cualquier nombre dentro de su espacio de nombres. Un servidor DNS responde a las consultas de una de las siguientes maneras:  
  
-   Si la respuesta está en la memoria caché, respuestas la consulta de la memoria caché.  
  
-   Si la respuesta es en una zona hospedada por el servidor DNS, respuestas la consulta de su zona. Una zona es una parte del árbol de DNS almacenado en un servidor DNS. Cuando un servidor DNS hospeda una zona, está autorizado para los nombres en esa zona (es decir, el servidor DNS puede responder a las consultas de nombres en la zona). Por ejemplo, un servidor que hospeda la zona contoso.com puede responder a las consultas de nombres en contoso.com.  
  
-   Si el servidor no puede responder a la consulta de la memoria caché o zonas, consulta otros servidores para la respuesta.  
  
Es importante comprender las características principales de DNS, como la delegación, recursiva la resolución de nombres y las zonas DNS integradas en Active Directory, ya que tienen un impacto directo en el diseño de la estructura lógica de Active Directory.  
  
Para obtener más información acerca de DNS y servicios de dominio de Active Directory (AD DS), consulta [DNS y AD DS](../../ad-ds/plan/DNS-and-AD-DS.md).  
  
## <a name="delegation"></a>Delegación  
Para que responder a las consultas sobre cualquier nombre de un servidor DNS, debe tener una ruta de acceso directa o indirecta a cada zona en el espacio de nombres. Estas rutas de acceso se crean mediante la delegación. Una delegación es un registro en una zona de principal que muestra un servidor de nombres que está autorizado para la zona en el siguiente nivel de la jerarquía. Delegación hacen que sea posible para los servidores en una zona para hacer referencia a los clientes a servidores en otras zonas. La siguiente ilustración muestra un ejemplo de la delegación.  
  
![Conceptos de DNS](../../media/Reviewing-DNS-Concepts/0c24b576-d41a-4e5d-ad3d-6be81e095835.gif)  
  
El servidor de la raíz DNS hospeda la zona de raíz representada como un punto (. ). La zona de raíz contiene una delegación en una zona en el siguiente nivel de la jerarquía, la zona de com. La delegación en la zona raíz indica al servidor de la raíz DNS que, para encontrar la zona de com, se debe en contacto con el servidor Com. De igual modo, la delegación en la zona de com indica al servidor de Com, para encontrar la zona contoso.com, se debe en contacto con el servidor de Contoso.  
  
> [!NOTE]  
> Una delegación utiliza dos tipos de registros. El registro de recursos de nombre del servidor (NS) proporciona el nombre de un servidor de autorización. Host (A) y registros de recursos de host (AAAA) proporcionan IP versión 4 (IPv4) y IP versión 6 (IPv6) las direcciones de un servidor de autorización.  
  
Este sistema de zonas y delegación crea un árbol jerárquico que representa el espacio de nombres DNS. Cada zona representa una capa de la jerarquía y cada delegación una rama del árbol.  
  
Mediante el uso de la jerarquía de las zonas y delegación, un servidor de raíz DNS encontrar cualquier nombre en el espacio de nombres DNS. La zona de raíz incluye delegación que conducen directa o indirectamente a todas las demás zonas de la jerarquía. Cualquier servidor que puede consultar el servidor de la raíz DNS puede utilizar la información de la delegación para buscar cualquier nombre en el espacio de nombres.  
  
## <a name="recursive-name-resolution"></a>Resolución de nombres recursiva  
Resolución de nombres recursiva es el proceso por el que un servidor DNS usa la jerarquía de las zonas y delegación para responder a las consultas que no está autorizado.  
  
En algunas configuraciones, servidores DNS incluyen sugerencias de raíz (es decir, una lista de nombres y direcciones IP) que puedan consultar los servidores de la raíz DNS. En otras configuraciones, servidores reenvían todas las consultas que no responden a otro servidor. Sugerencias de reenvío y raíz son dos métodos que pueden usar servidores DNS para resolver consultas para que no están autorizados.  
  
### <a name="resolving-names-by-using-root-hints"></a>Resolución de nombres mediante el uso de sugerencias de raíz  
Sugerencias de raíz habilitar cualquier servidor DNS buscar los servidores raíz DNS. Después de un servidor DNS localiza el servidor de la raíz DNS, puede resolver cualquier consulta para ese espacio de nombres. La siguiente ilustración describe cómo DNS resuelve un nombre mediante el uso de sugerencias de raíz.  
  
![Conceptos de DNS](../../media/Reviewing-DNS-Concepts/1c044845-b104-4262-a7af-474ba3558a85.gif)  
  
En este ejemplo, se producen los siguientes eventos:  
  
1.  Un cliente envía una consulta recursiva a un servidor DNS para solicitar la dirección IP que corresponde al nombre de ftp.contoso.com. Una consulta recursiva indica que el cliente quiere una respuesta definitiva a su consulta. La respuesta a la consulta recursiva debe ser una dirección válida o un mensaje que indica que no se encuentra la dirección.  
  
2.  Dado que el servidor DNS no está autorizado para el nombre y no tiene la respuesta en la memoria caché, el servidor DNS usa sugerencias de raíz para encontrar la dirección IP del servidor DNS raíz.  
  
3.  El servidor DNS usa una consulta iterativa para solicitar al servidor de la raíz DNS para resolver el nombre ftp.contoso.com. Una consulta iterativa indica que el servidor aceptará una referencia a otro servidor en lugar de una respuesta definitiva a la consulta. Dado que el nombre ftp.contoso.com termina con la etiqueta com, el servidor de raíz DNS devuelve una referencia al servidor Com que hospeda la zona de com.  
  
4.  El servidor DNS usa una consulta iterativa para solicitar al servidor Com para resolver el nombre ftp.contoso.com. Porque el nombre ftp.contoso.com termina con el nombre contoso.com, el servidor Com devuelve una referencia al servidor de Contoso que organiza la contoso.com la zona.  
  
5.  El servidor DNS usa una consulta iterativa para solicitar al servidor de Contoso para resolver el nombre ftp.contoso.com. El servidor de Contoso encuentra la respuesta en sus datos de la zona y, a continuación, devuelve la respuesta del servidor.  
  
6.  El servidor, a continuación, devuelve el resultado al cliente.  
  
### <a name="resolving-names-by-using-forwarding"></a>Resolución de nombres mediante el desvío de llamadas  
Permite a la resolución de nombres de ruta mediante determinados servidores en lugar de usar sugerencias de raíz. La siguiente ilustración describe cómo DNS resuelve un nombre mediante el uso de desvío de llamadas.  
  
![Conceptos de DNS](../../media/Reviewing-DNS-Concepts/05bc2eb0-1033-4e53-ae30-244fa247d000.gif)  
  
En este ejemplo, se producen los siguientes eventos:  
  
1.  Un cliente solicita un servidor DNS para el nombre ftp.contoso.com.  
  
2.  El servidor DNS reenvía la consulta a otro servidor DNS, conocido como servidor de envío.  
  
3.  Dado que el reenviador no está autorizado para el nombre y no tiene la respuesta en la memoria caché, usa sugerencias de raíz para encontrar la dirección IP del servidor DNS raíz.  
  
4.  El reenviador usa una consulta iterativa para solicitar al servidor de la raíz DNS para resolver el nombre ftp.contoso.com. Dado que el nombre ftp.contoso.com termina con el nombre com, el servidor de raíz DNS devuelve una referencia al servidor Com que hospeda la zona de com.  
  
5.  El reenviador usa una consulta iterativa para solicitar al servidor Com para resolver el nombre ftp.contoso.com. Porque el nombre ftp.contoso.com termina con el nombre contoso.com, el servidor Com devuelve una referencia al servidor de Contoso que organiza la contoso.com la zona.  
  
6.  El reenviador usa una consulta iterativa para solicitar al servidor de Contoso para resolver el nombre ftp.contoso.com. El servidor de Contoso encuentra la respuesta en sus archivos de la zona y, a continuación, devuelve la respuesta del servidor.  
  
7.  El reenviador, a continuación, devuelve el resultado al servidor DNS original.  
  
8.  El servidor DNS original, a continuación, devuelve el resultado al cliente.  
  


