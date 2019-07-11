---
title: Consideraciones de uso de memoria en la optimización del rendimiento de AD DS
description: Uso de memoria por el proceso de Lsass.exe en controladores de dominio que ejecutan Windows Server 2012 R2, 2016 y 2019.
ms.prod: windows-server-threshold
ms.technology: performance-tuning-guide
ms.topic: article
ms.author: v-tea; lindakup
author: Teresa-Motiv
ms.date: 7/3/2019
ms.openlocfilehash: dd33124d7d480f3670fa2781f567eaaa28abbce6
ms.sourcegitcommit: be243a92f09048ca80f85d71555ea6ee3751d712
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/11/2019
ms.locfileid: "67799787"
---
# <a name="memory-usage-considerations-for-ad-ds-performance-tuning"></a>Consideraciones de uso de memoria para la optimización de rendimiento de AD DS

En este artículo se describe algunos aspectos básicos de los servicio de subsistema de autoridad Local de seguridad (LSASS, también conocido como el proceso de Lsass.exe), procedimientos recomendados para la configuración de LSASS y expectativas de uso de memoria. En este artículo debe usarse como guía para el análisis de uso de memoria y el rendimiento de LSASS en controladores de dominio (DC). La información de este artículo puede ser útil si tiene preguntas acerca de cómo optimizar y configurar los servidores y controladores de dominio para optimizar este motor.  

LSASS es responsable de la administración de autenticación de dominio de seguridad local (LSA) de la entidad de certificación y la administración de Active Directory. LSASS controla la autenticación para el cliente y el servidor, y también controla el motor de Active Directory. LSASS es responsable de los siguientes componentes:  

- Autoridad de seguridad local
- Servicio NetLogon.
- Servicio de seguridad de administrador de cuentas (SAM)
- Servicio de servidor LSA
- Capa de Sockets seguros (SSL)
- Protocolo de autenticación Kerberos v5
- Protocolo de autenticación NTLM
- Otros paquetes de autenticación que carguen en LSA

Los servicios de base de datos de Active Directory (NTDSAI.dll) funcionan con el motor de almacenamiento Extensible (ESE, ESENT.dll).

Este es un diagrama visual de uso de memoria LSASS en un controlador de dominio:

![Diagrama de los componentes que usan memoria LSASS](media/domain-controller-lsass-memory-usage.png)  

La cantidad de memoria que utiliza LSASS en un controlador de dominio aumenta según el uso de Active Directory. Cuando se consultan los datos, se almacena en caché en memoria. Como resultado, es normal que vea LSASS con una cantidad de memoria que es mayor que el tamaño del archivo de base de datos de Active Directory (NTDS.dit).

Como se muestra en el diagrama, uso de memoria LSASS puede dividirse en varias partes, incluyendo la caché del búfer de base de datos de ESE, el almacén de versiones de ESE y otros. El resto de este artículo proporciona información sobre cada una de estas partes.

## <a name="ese-database-buffer-cache"></a>Caché de búfer de base de datos de ESE  
El uso de variables de memoria más grande en LSASS es la caché del búfer de base de datos ESE. El tamaño de la memoria caché puede oscilar desde menos de 1 MB y el tamaño de la base de datos completa. Dado que una caché mayor mejora el rendimiento, el motor de base de datos de Active Directory (ESENT) intenta mantener la memoria caché de mayor tamaño posible. Aunque el tamaño de la caché varía en función de la presión de memoria en el equipo, es el tamaño máximo de la caché del búfer de base de datos de ESE *sólo* limitado por la memoria física instalada en el equipo. Siempre y cuando no hay ningún otra presión de memoria, la memoria caché puede crecer hasta el tamaño del archivo de base de datos NTDS.dit de Active Directory. Más de la base de datos que puede almacenarse en caché, mejor será el rendimiento del controlador de dominio.  
  
> [!NOTE]
> Debido a la manera en que funciona el algoritmo de almacenamiento en caché de la base de datos, en un sistema de 64 bits en el que el tamaño de la base de datos es menor que la memoria RAM disponible, la memoria caché de la base de datos puede crecer mayor que el tamaño de la base de datos por 30 a 40 por ciento.

## <a name="ese-version-store"></a>Almacén de versiones de ESE

No hay uso de memoria variable por el almacén de versiones de ESE (la parte roja en el diagrama anterior). La cantidad de memoria usada depende de si tiene Windows Server 2019 o versiones anteriores de Windows.

- En versiones de Windows Server anteriores a Windows Server 2019, de forma predeterminada, que LSASS puede utilizar hasta aproximadamente 400 MB de memoria (según el número de CPU) en un equipo de 64 bits para la versión de ESE almacén. Para obtener más información sobre cómo se usa el almacén de versiones consulte el siguiente ASKDS blog Ryan RIES: [Llama la versión de Store y están todos fuera de depósitos](https://techcommunity.microsoft.com/t5/Ask-the-Directory-Services-Team/The-Version-Store-Called-and-They-8217-re-All-Out-of-Buckets/ba-p/400415).

- En Windows Server 2019, esto se ha simplificado y al NTDS de servicio se inicia primero, el tamaño del almacén de versión de ESE ahora se calcula como el 10% de memoria RAM física, con un mínimo de 400MB y un máximo de 4GB. Para detalles excelente sobre esto y solución de problemas de almacén de versión, vea otro blog excelente de Ryan Ries: [Profundización: Active Directory ESE versión Store cambia en el servidor de 2019](https://techcommunity.microsoft.com/t5/Ask-the-Directory-Services-Team/Deep-Dive-Active-Directory-ESE-Version-Store-Changes-in-Server/ba-p/400510).

## <a name="other-memory-use"></a>Otro uso de memoria

Por último, hay código, pilas, los montones y distintas estructuras de datos de tamaño fijo (por ejemplo, la memoria caché de esquema). La cantidad de memoria que utiliza LSASS puede variar, dependiendo de la carga en el equipo. A medida que aumenta el número de subprocesos en ejecución, también aumenta la cantidad de pilas de memoria. En promedio, LSASS usa 100 MB a 300 MB de memoria para estos componentes fijos. Cuando se instala una mayor cantidad de RAM, LSASS puede utilizar más memoria RAM y menos memoria virtual.

**Limitar o minimizar el número de programas en el controlador de dominio o agregar memoria RAM adicional cuando sea apropiado**

Para obtener un rendimiento óptimo, LSASS toma tanta RAM como sea posible en un DC determinado. LSASS cede ese RAM como pedir otros procesos. La idea es optimizar el rendimiento de LSASS mientras sigue teniendo en cuenta otros procesos que puedan ejecutarse en un equipo. La lista de programas para vigilar incluye agentes de supervisión. Algunos clientes tienen agentes independientes para distintas funciones de servidor que pueden consumir importantes recursos de memoria RAM. Algunos pueden emitir muchas consultas WMI, para lo que tenemos algunos detalles a continuación.

Por este motivo y para aumentar el rendimiento, es aconsejable limitar o reducir el número de programas en un controlador de dominio. Si no hay ninguna solicitud de memoria, LSASS usa esta memoria caché de la base de datos de Active Directory y, por tanto, lograr un rendimiento óptimo.

Cuando se tenga en cuenta que un controlador de dominio tiene problemas de rendimiento, vea también para los procesos con la utilización de memoria significativa. Estos pueden tener un problema que deba solucionar el problema. Pueden incluir componentes de Microsoft. Asegúrese de que se mantiene actualizado con las actualizaciones recientes de mantenimiento&mdash;Microsoft incluye soluciones para el uso excesivo de memoria como parte de las actualizaciones de calidad, que también pueden ayudar a su rendimiento de DC.

Hay funciones integradas de sistema operativo que pueden consumir importante RAM según el perfil de uso:

- **Servidor de archivos**. Los controladores de dominio también son servidores de archivos de recursos compartidos SYSVOL y Netlogon, directiva de grupo y scripts para los scripts de inicio de sesión y directivas de mantenimiento.
  Sin embargo, vemos clientes que usar controladores de dominio a otro contenido del archivo de servicio. El servidor de archivos SMB, a continuación, consumirá memoria RAM para realizar un seguimiento de los clientes activos, pero más importante, el contenido del archivo haría que la caché de archivos del sistema operativo también crecen y competir con la caché de base de datos de ESE de RAM.  

- **Consultas de WMI**. A menudo, las soluciones de supervisión que muchas consultas WMI. Una consulta individual puede ser económica ejecutar. A menudo es el volumen de llamadas que implica cierta sobrecarga, especialmente como las soluciones de supervisión extracción nuevos eventos de los distintos registros de eventos que administra Windows.  

  El registro de eventos producidos por el volumen de la mayoría es normalmente el registro de eventos de seguridad. Y esto es también el registro de eventos que los administradores de seguridad que van a recopilar, sobre todo desde controladores de dominio.  

  El servicio WMI usa un esquema de asignación de memoria dinámica que optimiza las consultas. Por lo tanto, el servicio WMI puede asignar una gran cantidad de memoria, compitiendo nuevo con la caché de base de datos ESE.  
