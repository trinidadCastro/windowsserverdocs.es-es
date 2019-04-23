---
ms.assetid: 4b844404-36ba-4154-aa5d-237a3dd644be
title: Introducción a la desduplicación de datos
ms.technology: storage-deduplication
ms.prod: windows-server-threshold
ms.topic: article
author: wmgries
manager: klaasl
ms.author: wgries
ms.date: 05/09/2017
ms.openlocfilehash: 4344108f96d14475c15a31bd1ab917e7fc78ef9f
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59860136"
---
# <a name="data-deduplication-overview"></a>Introducción a la desduplicación de datos

> Se aplica a: Windows Server (canal semianual), Windows Server 2016

## <a name="what-is-dedup"></a>¿Qué es desduplicación de datos?

Desduplicación de datos es una característica de Windows Server 2016 que pueden ayudar a reducir el impacto de los datos redundantes en los costos de almacenamiento. Cuando está habilitada, Desduplicación de datos optimiza el espacio libre en un volumen mediante el examen de los datos del volumen en busca de partes duplicadas en el volumen. Las partes duplicadas del conjunto de datos del volumen se almacenan una vez y, opcionalmente, se comprimen para un ahorro adicional. Desduplicación de datos optimiza redundancias sin poner en peligro la integridad o fidelidad de datos. Encontrará más información sobre cómo funciona Desduplicación de datos en la sección [¿Cómo funciona Desduplicación de datos?](understand.md#how-does-dedup-work) de la página [Información acerca de Desduplicación de datos](understand.md).

> [!Important]  
> [KB4025334](https://support.microsoft.com/kb/4025334) contiene un conjunto de correcciones de errores para Desduplicación de datos en el que se incluyen correcciones importantes de confiabilidad; te recomendamos encarecidamente que lo instales al usar Desduplicación de datos con Windows Server 2016.

## <a name="why-is-dedup-useful"></a>¿Por qué es útil desduplicación de datos?

Desduplicación de datos ayuda a los administradores de almacenamiento a reducir los costos asociados a los datos duplicados. Los grandes conjuntos de datos con frecuencia tienen **<u>una gran cantidad</u>** de duplicación, lo que aumenta los costos de almacenamiento de datos. Por ejemplo:

- Los recursos compartidos de archivos de usuario pueden tener varias copias de los mismos archivos o de archivos similares.
- Los invitados de virtualización pueden ser prácticamente idénticos de una máquina virtual a otra.
- Las instantáneas de copia de seguridad pueden tener diferencias menores de un día a otro.

Los ahorros de espacio que pueden obtenerse con Desduplicación de datos dependen del conjunto de datos o de la carga de trabajo en el volumen. Los conjuntos de datos con alta duplicación podrían ver tasas de optimización de hasta el 95 % o una reducción de 20 veces del uso del almacenamiento. En la tabla siguiente se destacan los ahorros típicos que produce la desduplicación para diferentes tipos de contenido:

| Escenario       | Contenido                                        | Ahorro de espacio típico |
|----------------|------------------------------------------------|-----------------------|
| Documentos de usuario | Documentos de Office, fotos, música, vídeos, etc.  | 30-50 %                |
| Recursos compartidos de implementación | Archivos binarios de software, archivos CAB, símbolos, etc. | 70-80 %                |
| Bibliotecas de virtualización | ISO, archivos de disco duro virtual, etc.  | 80-95 %                |
| Recursos compartidos de archivos generales | Todos los anteriores                           | 50-60 %                |

## <a id="when-can-dedup-be-used"></a>¿Cuando se usa la desduplicación de datos?  
<table>
    <tbody>
        <tr>
            <td style="text-align:center;min-width:150px;vertical-align:center;"><img src="media/overview-clustered-gpfs.png" alt="Illustration of file servers" /></td>
            <td style="vertical-align:top">
                <b>Servidores de archivos de propósito general</b><br />
Los servidores de archivos de uso general son los servidores de archivos destinados a un uso general que pueden contener alguno de los siguientes tipos de recursos compartidos: <ul>
                    <li>Recursos compartidos del equipo</li>
                    <li>Carpetas particulares de usuario</li>
                    <li><a href="https://technet.microsoft.com/library/dn265974.aspx">Carpetas de trabajo</a></li>
                    <li>Recursos compartidos de desarrollo de software</li>
                </ul>
Los servidores de archivos de uso general son un buen candidato para Desduplicación de datos debido a que los usuarios suelen tener muchas copias o versiones del mismo archivo. Los recursos compartidos de desarrollo de software se benefician de Desduplicación de datos porque muchos de los archivos binarios permanecen sin modificarse de una compilación a otra. 
            </td>
        </tr>
        <tr>
            <td style="text-align:center;min-width:150px;vertical-align:center;"><img src="media/overview-vdi.png" alt="Illustration of VDI servers" /></td>
            <td style="vertical-align:top">
                <b>Implementaciones virtualizadas de Desktop Infrastructure (VDI)</b><br />
Los servidores VDI, como <a href="https://technet.microsoft.com/library/cc725560.aspx">Servicios de Escritorio remoto</a>, proporcionan una opción ligera para que las organizaciones aprovisionen de escritorios a los usuarios. Hay muchas razones para que una organización dependa de esa tecnología: <ul>
                    <li><b>Implementación de la aplicación</b>: Puede implementar rápidamente las aplicaciones en toda la empresa. Esto es especialmente útil cuando tiene aplicaciones que se actualizan con frecuencia, se usan con poca frecuencia o son difíciles de administrar.</li>
                    <li><b>Consolidación de aplicaciones</b>: Al instalar y ejecutar aplicaciones desde un conjunto de máquinas virtuales administradas centralmente, se elimina la necesidad de actualizar las aplicaciones en los equipos cliente. Esta opción también reduce la cantidad de ancho de banda de red que se necesita para acceder a las aplicaciones.</li>
                    <li><b>Acceso remoto</b>: Los usuarios pueden tener acceso a aplicaciones empresariales desde dispositivos como equipos domésticos, quioscos, hardware de baja potencia y sistemas operativos distintos de Windows.</li>
                    <li><b>Acceso a sucursales</b>: Las implementaciones de VDI pueden proporcionar un mejor rendimiento de la aplicación para la rama los trabajadores de oficina que necesitan acceso a almacenes de datos centralizados. A veces las aplicaciones con un uso intensivo de datos no tienen protocolos cliente/servidor optimizados para conexiones de baja velocidad.</li>
                </ul>
Las implementaciones de VDI son excelentes candidatas para Desduplicación de datos porque los discos duros virtuales que llevan los Escritorios remotos a los usuarios son prácticamente idénticos. Además, Desduplicación de datos puede ayudarle con los arranques simultáneos de VDI (lo que se conoce como *boot storm*), que reduce el rendimiento de almacenamiento cuando muchos usuarios inician sesión en el escritorio al mismo tiempo a la vez al comienzo del día.
            </td>
        </tr>
        <tr>
            <td style="text-align:center;min-width:150px;vertical-align:center;"><img src="media/overview-backup.png" alt="Illustration of backup applications" /></td>
            <td style="vertical-align:top">
                <b>Destinos de copia de seguridad, como las aplicaciones virtualizadas de copia de seguridad</b><br />
Las aplicaciones de copia de seguridad, como <a href="https://technet.microsoft.com/library/hh758173.aspx">Microsoft Data Protection Manager (DPM)</a>, son candidatas perfectas para Desduplicación de datos debido a una duplicación significativa entre las instantáneas de copia de seguridad.
            </td>
        </tr>
        <tr>
            <td style="text-align:center;min-width:150px;vertical-align:center;"><img src="media/overview-other.png" alt="Illustration of other workloads" /></td>
            <td style="vertical-align:top">
                <b>Otras cargas de trabajo</b><br />
                [Otras cargas de trabajo también pueden ser candidatas perfectas para Desduplicación de datos](install-enable.md#enable-dedup-candidate-workloads).
            </td>
        </tr>
    </tbody>
</table>
