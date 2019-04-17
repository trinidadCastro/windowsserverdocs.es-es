---
title: Novedades de Windows Server, versión 1709
description: ¿Cuáles son las nuevas características de proceso, identidad, administración, automatización, redes, seguridad y almacenamiento?
ms.prod: windows-server-threshold
ms.technology: server-general
ms.topic: article
author: coreyp-at-msft
ms.author: coreyp
ms.localizationpriority: medium
ms.openlocfilehash: 1d63721dde484756e67b68bcff078257c130ae36
ms.sourcegitcommit: 2ffd664a771421231a583d0b90827e922ed47ae3
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/15/2018
ms.locfileid: "4646132"
---
# Novedades de Windows Server, versión 1709

>Se aplica a: WindowsServer (canal semianual)

<img src="../media/landing-icons/new.png" style='float:left; padding:.5em;' alt="Icon showing a newspaper">&nbsp;El contenido de esta sección describe las novedades y los cambios de Windows Server, versión 1709. Las nuevas características y los cambios que se muestran aquí son los que probablemente tengan un mayor impacto al trabajar con esta versión. Consulta también [Windows Server, versión 1709](https://blogs.technet.microsoft.com/windowsserver/2017/08/24/sneak-peek-1-windows-server-version-1709/).
   

## Nueva cadencia de versiones

A partir de esta versión, tienes dos opciones para recibir actualizaciones de características de Windows Server:
- **Canal de mantenimiento a largo plazo (LTSC)**: esta es la opción habitual con 5 años de soporte estándar y 5 años de soporte extendido. Tienes la opción de actualizar a la próxima versión LTSC cada 2 o 3 años de la misma manera que se ha ofrecido soporte durante los últimos 20 años.
- **Canal semianual (SAC)**: es una ventaja de Software Assurance y ofrece soporte completo en la producción. La diferencia es que se ofrece soporte durante 18meses y habrá una nueva versión cada seis meses.

Los canales de lanzamiento se resumen en la tabla siguiente.

|   | Canal semianual | Canal de mantenimiento a largo plazo |
| ------------- | ------------- | ------------ |
| Cadencia de lanzamientos  | Dos veces al año (primavera y otoño)  | Cada 2 o 3 años |
| Programación de soporte  | Soporte de producción estándar durante 18meses  | Soporte estándar durante 5 años + Soporte extendido durante 5 años |
| Disponibilidad  | Software Assurance o Azure (hospedado en la nube)  | Todos los canales |
| Convenciones de nomenclatura  | Windows Server, versión AAMM  | Windows Server AAAA |

Para obtener más información, consulta [Introducción al Canal semianual de WindowsServer](https://docs.microsoft.com/windows-server/get-started/semi-annual-channel-overview).

## Microservicios y contenedores de aplicaciones

- La imagen del contenedor de Server Core se ha optimizado aún más para escenarios de elevación y desplazamiento donde se pueden migrar bases de código existentes o aplicaciones en contenedores con cambios mínimos, y también es un 60% más reducida. 
- La imagen de contenedor de Nano Server es casi un 80% más reducida.
    - En el Canal semianual de Windows Server, Nano Server como imagen de sistema operativo base de contenedor disminuye de 390MB a 80MB.
- Contenedores Linux con aislamiento de Hyper-V 

Para obtener más información, consulta [Cambios en NanoServer en el próximo lanzamiento de WindowsServer](https://docs.microsoft.com/windows-server/get-started/nano-in-semi-annual-channel) y [Windows Server, versión 1709 para desarrolladores](https://blogs.technet.microsoft.com/windowsserver/2017/09/13/sneak-peek-3-windows-server-version-1709-for-developers/).

## Administración moderna

Echa un vistazo a [Proyecto Honolulu](https://docs.microsoft.com/windows-server/manage/honolulu/honolulu) para ofrecer una experiencia simplificada, integrada y segura con el fin de ayudar a los administradores de TI a administrar escenarios básicos de solución de problemas, configuración y mantenimiento.  Proyecto Honolulu incluye la próxima generación de herramientas con una interfaz simplificada, integrada, segura y extensible.
Proyecto Honolulu incluye una experiencia de administración intuitiva y totalmente nueva para administrar equipos, servidores de Windows, clústeres de conmutación por error, así como infraestructuras hiperconvergidas en función de Almacenamiento de espacios directos, lo que reduce los costes operativos.

## Cálculo

**Contenedor Nano y contenedor Server Core**: ante todo, este lanzamiento está destinado a impulsar la innovación de aplicaciones. Nano Server o Nano como host está en desuso y se ha sustituido por el contenedor Nano, que es Nano ejecutándose como una imagen de contenedor. 

Para obtener más información sobre los contenedores, consulta [Introducción a las redes contenedor](https://docs.microsoft.com/windows-server/networking/sdn/technologies/containers/container-networking-overview).

Host de **Server Core como contenedor** (e infraestructura) proporciona mejor flexibilidad, densidad y rendimiento para las aplicaciones existentes en un proceso de modernización y crea la marca de nuevas aplicaciones desarrolladas que ya se han desarrollado con el modelo de nube.

**Orden de iniciar VM** también se ha mejorado con reconocimiento de sistema operativo y aplicación, incluyendo desencadenadores mejorados para cuando se considera que una máquina virtual se ha iniciado antes de iniciar la siguiente.

**Soporte de memoria de clase de almacenamiento para VM** permite la creación de volúmenes de acceso directo con formato NTFS en DIMM no volátiles y la exposición a VM de Hyper-V. Esto permite que las máquinas virtuales de Hyper-V saquen partido de las ventajas de rendimiento de latencia baja de dispositivos de memoria de clase de almacenamiento.

**Memoria persistente virtualizada (vPMEM)** se habilita mediante la creación de un archivo VHD (.vhdpmem) en un volumen de acceso directo en un host, añadiendo un controlador vPMEM a una VM y añadiendo el dispositivo creado (.vhdpmem) a una VM. El uso de archivos vhdpmem en volúmenes de acceso directo en un host para respaldar vPMEM permite la flexibilidad de asignación y saca partido del modelo de administración conocido para añadir discos a máquinas virtuales.

**Almacenamiento de contenedor: volúmenes de datos persistentes en volúmenes compartidos de clúster (CSV)**. En Windows Server, versión 1709, así como Windows Server 2016 con las últimas actualizaciones, hemos agregado soporte de contenedores para tener acceso a los volúmenes de datos persistentes ubicados en CSV, incluidos CSV en Espacios de almacenamiento directos. Esto ofrece acceso persistente del contenedor de aplicaciones al volumen sin importar el nodo de clúster en el que se ejecuta la instancia de contenedor. Para obtener más información, consulta [Soporte de almacenamiento de contenedor con Volúmenes compartidos de clúster (CSV), Espacios de almacenamiento directos (S2D), Asignación global de SMB](https://blogs.msdn.microsoft.com/clustering/2017/08/10/container-storage-support-with-cluster-shared-volumes-csv-storage-spaces-direct-s2d-smb-global-mapping/).

**Almacenamiento de contenedor: volúmenes de datos persistentes con asignación global de SMB**. En Windows Server, versión 1709 hemos agregado soporte para asignar un uso compartido de archivos de SMB a una letra de unidad de un contenedor; esto se denomina asignación global de SMB. Esta unidad asignada es accesible para todos los usuarios en el servidor local para que dicho contenedor de E/S del volumen de datos pueda atravesar la unidad montada hacia el uso compartido de archivos subyacente. Para obtener más información, consulta [Soporte de almacenamiento de contenedor con Volúmenes compartidos de clúster (CSV), Espacios de almacenamiento directos (S2D), Asignación global de SMB](https://blogs.msdn.microsoft.com/clustering/2017/08/10/container-storage-support-with-cluster-shared-volumes-csv-storage-spaces-direct-s2d-smb-global-mapping/).

**Formato de archivo de configuración de máquina virtual (actualizado)**. Se ha agregado un archivo (.vmgs) adicional para máquinas virtuales con una versión de la configuración de 8.2 y superior. VMGS significa estado de invitado VM y es un nuevo archivo interno que incluye el estado del dispositivo que anteriormente formaba parte del archivo de estado en tiempo de ejecución de máquina virtual.

## Seguridad y control

**Cifrado de red** te permite cifrar rápidamente segmentos de red en la infraestructura de red definida por software para cumplir con las necesidades de seguridad y cumplimiento.

La opción **Servicio de protección de host (HGS)** como máquina virtual blindada está habilitada. Antes de este lanzamiento, se recomendaba implementar un clúster físico de 3 nodos. Aunque esto garantiza que el entorno HGS no se vea afectado por un administrador, a menudo su coste es prohibitivo.

**Linux como máquina virtual blindada** ya es compatible.

Para obtener más información, consulta [Información general sobre máquinas virtuales blindadas y tejido protegido](https://docs.microsoft.com/windows-server/virtualization/guarded-fabric-shielded-vm/guarded-fabric-and-shielded-vms).

**Vulnerabilidad SMBLoris** Se ha solucionado un problema, conocido como “SMBLoris”, que podría provocar denegaciones del servicio.

## Almacenamiento

**Réplica de almacenamiento**: la protección de recuperación ante desastres añadida por Réplica de almacenamiento en Windows Server 2016 se ha expandido para incluir:
- **Conmutación por error de prueba**: la opción para montar el almacenamiento de destino ya es posible a través de la característica Conmutación por error de prueba. Puedes montar una instantánea del almacenamiento replicado en los nodos de destino temporalmente para realizar pruebas o copias de seguridad.  Para obtener más información, consulta [Preguntas frecuentes acerca de Réplica de almacenamiento](https://aka.ms/srfaq). 
- **Soporte del proyecto Honolulu**: el soporte de la administración gráfica de la replicación servidor a servidor ya está disponible en el Proyecto Honolulu. Esto elimina la necesidad de usar PowerShell para administrar una carga de trabajo de protección ante desastres comunes.

**SMB**: 
- **Eliminación de autenticación de invitados y SMB1**: Windows Server, versión 1709 ya no instala el cliente SMB1 y el servidor de forma predeterminada. Además, la capacidad de autenticar como invitado en SMB2 y versiones posteriores está desactivada de forma predeterminada. Para obtener más información, revisa [SMBv1 no está instalado de forma predeterminada en Windows 10, versión 1709 y Windows Server, versión 1709](https://support.microsoft.com/help/4034314/smbv1-is-not-installed-by-default-in-windows-10-rs3-and-windows-server). 

- **Compatibilidad y seguridad de SMB2/SMB3**: se añadieron opciones adicionales de compatibilidad de seguridad y aplicaciones, incluida la capacidad de desactivar bloqueos oportunistas en SMB2+ para aplicaciones heredadas, además de requerir inicio de sesión o cifrado en función de la base por conexión de un cliente. Para obtener más información, revisa la Ayuda del módulo de SMBShare PowerShell.

**Desduplicación de datos**: 
- **La Desduplicación de datos ahora admite ReFS**: ya no tienes que elegir entre las ventajas de un sistema de archivos moderno con ReFS y la Desduplicación de datos, ya que ahora puedes habilitar la Desduplicación de datos siempre que puedas habilitar ReFS. Aumenta la eficacia del almacenamiento hasta un 95% con ReFS.
- **API de DataPort para entrada/salida optimizadas de volúmenes desduplicados**: los desarrolladores pueden ahora sacar partido de la Desduplicación de datos de información con el fin de almacenar datos de forma eficaz para mover datos entre volúmenes, servidores y clústeres.

## Servicios de Escritorio remoto (RDS)

**RDS está integrado con Azure AD**, para que los clientes puedan sacar partido de las directivas de Acceso condicionales, Autenticación multifactor, Autenticación integrada con otras aplicaciones SaaS con Azure AD y mucho más. Para obtener más información, consulta [Integrar Azure AD Domain Services con la implementación de RDS](https://docs.microsoft.com/windows-server/remote/remote-desktop-services/rds-azure-adds).

>[!TIP]
>Para echar un vistazo a otros cambios increíbles que llegan a RDS, consulta [Servicios de Escritorio remoto: actualizaciones y próximas innovaciones](https://blogs.technet.microsoft.com/enterprisemobility/2017/09/20/first-look-at-updates-coming-to-remote-desktop-services/)

## Redes

**Malla de enrutamiento de Docker** es compatible. La malla de enrutamiento de entrada forma parte del [modo enjambre](https://docs.docker.com/engine/swarm/), la solución de orquestación integrada de Docker para contenedores. Para obtener más información, consulta [Malla de enrutamiento de Docker disponible con Windows Server, versión 1709](https://blogs.technet.microsoft.com/virtualization/2017/09/26/dockers-ingress-routing-mesh-available-with-windows-server-version-1709/).

**Nuevas características de Docker** disponibles. Para obtener más información, consulta [Novedades increíbles para Docker con Windows Server 1709](https://blog.docker.com/2017/09/docker-windows-server-1709/).

**Redes de Windows en paridad con Linux para Kubernetes**: Windows funciona del mismo modo con Linux en lo que respecta a las redes. Los clientes pueden implementar clústeres de Kubernetes de sistemas operativos mixtos en cualquier entorno, incluidas pilas de nube de terceros, locales y Azure con las mismas topologías y tipos primitivos de redes compatibles con Linux sin la necesidad de extensiones de conmutador o soluciones alternativas.

**Pila de red principal**: se han mejorado varias características de la pila de red principal. Para obtener más información sobre estas características, consulta [Características de pila de red principal en Creators Update para Windows 10](https://blogs.technet.microsoft.com/networking/2017/07/13/core-network-stack-features-in-the-creators-update-for-windows-10/).
- **TCP Fast Open (TFO)**: soporte de TFO se ha añadido para optimizar el proceso de protocolo de enlace TCP de 3 vías. TFO establece una cookie TFO segura en la primera conexión con un proceso de protocolo de enlace de 3 vías estándar.  Las conexiones posteriores al mismo servidor usan la cookie TFO en lugar de un proceso de protocolo de enlace de 3 vías para conectar con un tiempo de ida y vuelta cero.
- **CUBIC**: implementación nativa de Windows experimental de CUBIC, un algoritmo de control de congestión TCP disponible. Los siguientes comandos habilitan o deshabilitan CUBIC, respectivamente.

    ```
    netsh int tcp set supplemental template=internet congestionprovider=cubic
    netsh int tcp set supplemental template=internet congestionprovider=compound
    ```

- **Ajuste automático de la ventana de recepción**: la lógica del ajuste automático de TCP calcula el parámetro "ventana de recepción" de una conexión TCP.  Las conexiones de retraso largo o de alta velocidad necesitan este algoritmo para lograr características de buen rendimiento.  En esta versión, se ha modificado el algoritmo para usar una función gradual para convergir en el valor máximo de recepción de ventana de una determinada conexión.
- **API de estadísticas de TCP**: se introduce una nueva API denominada SIO_TCP_INFO.  SIO_TCP_INFO permite a los desarrolladores consultar información enriquecida sobre conexiones TCP con una opción de socket.
- **IPv6**: hay varias mejoras en IPv6 en esta versión.
    - Soporte de **RFC 6106**: RFC 6106 que permite la configuración de DNS a través de anuncios de enrutador (RA). Puedes utilizar el siguiente comando para habilitar o deshabilitar el soporte de RFC 6106:

    ```
    netsh int ipv6 set interface <ifindex> rabaseddnsconfig=<enabled | disabled>
    ```

    - **Etiquetas de flujo**: a partir de Creators Update, los paquetes de salida de TCP y UDP sobre IPv6 tienen este campo establecido en un valor hash de 5 tuplas (Src IP, Dst IP, Src Port, Dst Port).  Esto permitirá que los centros de datos de solo IPv6 realicen un equilibrio de carga o clasificación de flujos de forma más eficaz. Para habilitar etiquetas de flujo:

    ```
    netsh int ipv6 set flowlabel=[disabled|enabled] (enabled by default)
    netsh int ipv6 set global flowlabel=<enabled | disabled>
    ```

    - **ISATAP y 6to4**: como un paso hacia un desuso futuro, Creators Update tendrá estas tecnologías deshabilitadas de forma predeterminada.
- **Detección de puertas de enlace inactivas (DGD)**: el algoritmo DGD transfiere automáticamente conexiones a otra puerta de enlace cuando no se puede alcanzar la puerta de enlace actual. En esta versión, se ha mejorado el algoritmo periódicamente para volver a probar el entorno de red.
- [Test-NetConnection](https://technet.microsoft.com/itpro/powershell/windows/nettcpip/test-netconnection) es un cmdlet integrado en Windows PowerShell que realiza una variedad de diagnósticos de red.  En esta versión, hemos mejorado el cmdlet para proporcionar información detallada sobre la selección de la ruta, así como la selección de direcciones de origen.

**Redes definidas por software**

- **Cifrado de red virtual** es una nueva característica que proporciona la capacidad para cifrar el tráfico de red virtual entre máquinas virtuales que se comunican entre sí dentro de subredes que están marcadas con "Cifrado habilitado". Esta característica utiliza la Seguridad de la capa de transporte de datagrama (DTLS) en la subred virtual para cifrar los paquetes.  DTLS proporciona protección contra interceptaciones, alteraciones y falsificaciones realizadas por cualquier persona con acceso a la red física.
 
**VPN de Windows 10**

- **Túneles de infraestructura de inicio de sesión previo**. De forma predeterminada, VPN de Windows 10 no crea automáticamente túneles infraestructura cuando los usuarios no han iniciado sesión en su equipo o dispositivo. Puedes configurar VPN de Windows 10 para crear automáticamente Túneles de infraestructura de inicio de sesión previo mediante la característica de túnel de dispositivo (inicio de sesión previo) en el perfil VPN.
- **Administración de equipos y dispositivos remotos**.  Puedes administrar los clientes de VPN de Windows 10 mediante la configuración de la característica de túnel de dispositivo (inicio de sesión previo) en el perfil VPN. Además, debes configurar la conexión VPN para registrar dinámicamente las direcciones IP que se asignan a la interfaz VPN con servicios DNS internos.
- **Especificar puertas de enlace de inicio de sesión previo**. Puedes especificar puertas de enlace de inicio de sesión previo con la característica de túnel de dispositivo (inicio de sesión previo) en el perfil VPN, combinada con filtros de tráfico para controlar qué sistemas de administración de la red corporativa son accesibles mediante el túnel de dispositivo.

