---
title: Consideraciones sobre el uso de memoria en la optimización del rendimiento de AD DS
description: Uso de memoria por parte del proceso Lsass. exe en los controladores de dominio que ejecutan Windows Server 2012 R2, 2016 y 2019.
ms.prod: windows-server
ms.technology: performance-tuning-guide
ms.topic: article
ms.author: v-tea; lindakup
author: teresa-motiv
ms.date: 7/3/2019
ms.openlocfilehash: cceabd73a3064ff82cfe1d3c353ea63574f5feff
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/08/2020
ms.locfileid: "80851888"
---
# <a name="memory-usage-considerations-for-ad-ds-performance-tuning"></a>Consideraciones sobre el uso de memoria para el ajuste del rendimiento AD DS

En este artículo se describen algunos aspectos básicos del Servicio de subsistema de autoridad de seguridad local (LSASS) (LSASS, también conocido como proceso Lsass. exe), procedimientos recomendados para la configuración de LSASS y expectativas de uso de memoria. Este artículo debe usarse como guía en el análisis del rendimiento de LSASS y el uso de memoria en los controladores de dominio (DC). La información de este artículo puede ser útil si tiene preguntas sobre cómo optimizar y configurar servidores y controladores de datos para optimizar este motor.  

LSASS es responsable de la administración de la autenticación de dominio de la autoridad de seguridad local (LSA) y de la administración de Active Directory. LSASS controla la autenticación del cliente y del servidor, y también rige el motor de Active Directory. LSASS es responsable de los siguientes componentes:  

- Autoridad de seguridad local
- Servicio NetLogon
- Servicio del administrador de cuentas de seguridad (SAM)
- Servicio servidor LSA
- Capa de sockets seguros (SSL)
- Protocolo de autenticación Kerberos V5
- protocolo de autenticación NTLM
- Otros paquetes de autenticación que se cargan en LSA

Los servicios de Active Directory Database (NTDSAI. dll) funcionan con el motor de almacenamiento extensible (ESE, ESENT. dll).

A continuación se muestra un diagrama visual del uso de memoria de LSASS en un controlador de dominio:

![Diagrama de los componentes que utilizan la memoria de LSASS](media/domain-controller-lsass-memory-usage.png)  

La cantidad de memoria que usa LSASS en un controlador de dominio aumenta de acuerdo con el uso de Active Directory. Cuando se consultan los datos, se almacenan en la memoria caché. Como resultado, es normal ver LSASS usando una cantidad de memoria mayor que el tamaño del archivo de base de datos de Active Directory (NTDS. DIT).

Como se muestra en el diagrama, el uso de memoria de LSASS se puede dividir en varias partes, como la caché de búfer de la base de datos ESE, el almacén de versiones de ESE y otros. En el resto de este artículo se proporciona información sobre cada una de estas partes.

## <a name="ese-database-buffer-cache"></a>Caché de búfer de base de datos ESE  
El mayor uso de memoria de la variable en LSASS es la memoria caché del búfer de la base de datos ESE. El tamaño de la memoria caché puede oscilar entre menos de 1 MB y el tamaño de la base de datos completa. Dado que una memoria caché mayor mejora el rendimiento, el motor de base de datos para Active Directory (ESENT) intenta mantener la memoria caché lo más grande posible. Aunque el tamaño de la memoria caché varía en función de la presión de memoria en el equipo, el tamaño máximo de la memoria caché del búfer de la base de datos ESE *solo* está limitado por la RAM física instalada en el equipo. Siempre que no haya ninguna otra presión de memoria, la memoria caché puede aumentar hasta el tamaño del archivo de base de datos NTDS. dit Active Directory. Cuanto mayor sea la base de datos que se puede almacenar en caché, mejor será el rendimiento del controlador de dominio.  
  
> [!NOTE]
> Debido a la manera en que funciona el algoritmo de almacenamiento en caché de base de datos, en un sistema de 64 bits en el que el tamaño de la base de datos es menor que el de la RAM disponible, la caché de base de datos puede aumentar el tamaño de la base de datos entre 30 y 40 por ciento.

## <a name="ese-version-store"></a>Almacén de versiones de ESE

El almacén de la versión de ESE es el uso de memoria variable (la parte roja del diagrama anterior). La cantidad de memoria que se usa depende de si tiene Windows Server 2019 o versiones anteriores de Windows.

- En las versiones de Windows Server anteriores a Windows Server 2019, de forma predeterminada, LSASS puede usar hasta aproximadamente 400 MB de memoria (en función del número de CPU) en un equipo de 64 bits para el almacén de versiones de ESE. Para obtener más información sobre cómo se usa el almacén de versiones, vea la siguiente entrada de blog de preguntar A servicios de Ryan Ries: [el almacén de versiones denominado y todos los cubos](https://techcommunity.microsoft.com/t5/Ask-the-Directory-Services-Team/The-Version-Store-Called-and-They-8217-re-All-Out-of-Buckets/ba-p/400415).

- En Windows Server 2019, se simplifica y cuando se inicia por primera vez el servicio NTDS, el tamaño del almacén de versiones de ESE ahora se calcula como el 10% de la RAM física, con un mínimo de 400 MB y un máximo de 4 GB. Para obtener más información sobre esta solución de problemas y el almacén de versiones, vea otro blog de Ryan Ries: [profundización: Active Directory cambios del almacén de versiones de ese en el servidor 2019](https://techcommunity.microsoft.com/t5/Ask-the-Directory-Services-Team/Deep-Dive-Active-Directory-ESE-Version-Store-Changes-in-Server/ba-p/400510).

## <a name="other-memory-use"></a>Otro uso de memoria

Por último, hay código, pilas, montones y varias estructuras de datos de tamaño fijo (por ejemplo, la caché de esquema). La cantidad de memoria que usa LSASS puede variar en función de la carga del equipo. A medida que aumenta el número de subprocesos en ejecución, se incrementa el número de pilas de memoria. En promedio, LSASS usa 100 MB a 300 MB de memoria para estos componentes fijos. Cuando se instala una cantidad mayor de memoria RAM, LSASS puede usar más RAM y menos memoria virtual.

**Limitar o minimizar el número de programas en el controlador de dominio o agregar RAM adicional cuando sea necesario**

Para obtener un rendimiento óptimo, LSASS toma tanta RAM como sea posible en un controlador de dominio determinado. LSASS renuncia a esa RAM a medida que otros procesos la solicitan. La idea es optimizar el rendimiento de LSASS mientras se sigue teniendo en cuenta otros procesos que podrían ejecutarse en un equipo. La lista de programas que se van a supervisar incluye agentes de supervisión. Algunos clientes tienen agentes independientes para las distintas funciones de servidor que pueden consumir recursos de RAM considerables. Algunos pueden emitir muchas consultas de WMI, para las que tenemos algunos detalles a continuación.

Debido a esto y para aumentar el rendimiento, es recomendable limitar o minimizar el número de programas en un controlador de dominio. Si no hay ninguna solicitud de memoria, LSASS usa esta memoria para almacenar en caché el Active Directory base de datos y, por tanto, lograr un rendimiento óptimo.

Si observa que un controlador de dominio tiene problemas de rendimiento, vea también los procesos con un uso significativo de la memoria. Puede que se deba a un problema que necesita para solucionar los problemas. Pueden incluir componentes de Microsoft. Asegúrese de mantenerse al día con las actualizaciones de servicio recientes&mdash;Microsoft incluye soluciones para el uso excesivo de memoria como parte de las actualizaciones de calidad, que también pueden ayudar al rendimiento del controlador de dominio.

Hay instalaciones de sistema operativo integradas que pueden consumir una gran cantidad de RAM según el perfil de uso:

- **Servidor de archivos**. Los controladores de comandos son también servidores de archivos para los recursos compartidos de SYSVOL y Netlogon, servicios y directivas de grupo de mantenimiento de directivas y scripts de inicio o inicio de sesión.
  Sin embargo, vemos que los clientes usan controladores de archivos para atender a otro contenido de archivo. El servidor de archivos SMB consumiría RAM para realizar el seguimiento de los clientes activos, pero lo más importante es que el contenido del archivo haría que la caché de archivos del sistema operativo crezca y competir con la memoria caché de la base de datos ESE para la RAM.  

- **Consultas WMI**. Las soluciones de supervisión suelen realizar muchas consultas de WMI. Una consulta individual puede ser barata de ejecutarse. A menudo, es el volumen de llamadas que conlleva cierta sobrecarga, especialmente cuando las soluciones de supervisión extraen nuevos eventos de los distintos registros de eventos que administra Windows.  

  El registro de eventos que genera el mayor volumen suele ser el registro de eventos de seguridad. Y este es también el registro de eventos que los administradores de seguridad desean recopilar, especialmente de los controladores de sesión.  

  El servicio WMI utiliza un esquema de asignación de memoria dinámica que optimiza las consultas. Por lo tanto, el servicio WMI puede asignar una gran cantidad de memoria y competir de nuevo con la caché de la base de datos ESE.  
