---
title: Caso práctico de Windows Admin Center SDK - Thomas Krenn
description: Caso práctico de Windows Admin Center SDK - Thomas Krenn
ms.technology: extend
ms.topic: article
author: daniellee-msft
ms.author: jol
ms.date: 06/24/2019
ms.localizationpriority: medium
ms.prod: windows-server-threshold
ms.openlocfilehash: 93b8a450aa86a454ec6febd349fcaa35df590266
ms.sourcegitcommit: 3be280c8638214857dc355b201eb56a04499a5e5
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 06/26/2019
ms.locfileid: "67396765"
---
# <a name="thomas-krennag-extension"></a>Extensión de Thomas Krenn.AG

## <a name="intuitive-server-and-storage-health-management"></a>Administración intuitiva de mantenimiento de servidores y almacenamiento

La extensión de Thomas Krenn.AG Windows Admin Center está diseñada específicamente para la alta disponibilidad, 2 nodos [S2D Micro-Cluster](https://www.thomas-krenn.com/en/products/application/software-defined-storage/s2d-micro-cluster.html) dispositivo. La interfaz gráfica fácil de usar web visualiza el estado de mantenimiento de un Micro-del clúster a través de un panel simple y le permite profundizar en los dispositivos de almacenamiento, interfaces de red o todo el clúster para ver más detalles.

La extensión ofrece acceso intuitivo a información suelen ser necesario para las llamadas de servicio y soporte técnico de primer nivel, como números de serie, las versiones de software, uso del almacenamiento y mucho más. Está diseñado para ser útil para los administradores que no tienen ninguna experiencia previa con infraestructura hiperconvergida de Windows Server.

Una parte de la información disponible es:
- Información general acerca de los nodos de Micro y el clúster de Micro
- Sistema operativo o el estado del dispositivo de arranque
- Capacidad de disco duro y almacenamiento en caché de estado SSD
- Eventos de clúster
- Estado de la red y la información

Utilice el panel para determinar el estado de mantenimiento y la información del sistema importantes como números de serie, modelo, versión del sistema operativo y utilización del clúster. Además, el ventilador, NIC y el estado general de hardware de nodo se muestran en el panel también.

![Extensión de Thomas Krenn](../../media/extend-case-study-thomas-krenn/thomas-krenn-1.png)

Puede profundizar en los dispositivos de almacenamiento para ver los números de serie, el estado de inteligentes y utilización de la capacidad. Dispositivos de arranque también muestran desgaste de indicadores, vuelve a asignar los sectores y potencia a tiempo, que ofrecen la mejor indicación del estado SSD.

![Extensión de Thomas Krenn](../../media/extend-case-study-thomas-krenn/thomas-krenn-2.png)

El icono de estado del clúster se expande para mostrar un resumen de los detalles operativos del clúster.

![Extensión de Thomas Krenn](../../media/extend-case-study-thomas-krenn/thomas-krenn-3.png)

Después de testigo de nube de Azure del este clúster Micro estuvo disponible durante una noche toda, un solo vistazo es suficiente para identificar el problema. Al hacer clic en "Notificaciones" inmediatamente, enumera los eventos relevantes para la corrección rápida. Eventos de clúster se localiza y determina el idioma del sistema operativo base. La propia extensión es compatible con inglés y alemán.

![Extensión de Thomas Krenn](../../media/extend-case-study-thomas-krenn/thomas-krenn-4.png)

Información de red también está disponible.

![Extensión de Thomas Krenn](../../media/extend-case-study-thomas-krenn/thomas-krenn-5.png)

Según los comentarios recibidos, hemos implementado también "Oscuro modo" disponible en Windows Admin Center v1904. Se trata de un relajante en centros de datos oscuro y mal tubo gabinetes y armarios. También hace que Windows Admin Center sea más accesible mediante la reducción deslumbramiento para que los administradores con ciertas discapacidades visuales.

![Extensión de Thomas Krenn](../../media/extend-case-study-thomas-krenn/thomas-krenn-6.png)

Thomas Krenn obtienen inmediatamente la facilidad de uso y la accesibilidad para que los administradores no entrenados sería clave para una experiencia excelente del cliente para infraestructuras hiperconvergidas en el mercado de pequeñas y medianas empresas. Extensión de Micro-Cluster del Krenn Thomas complementa perfectamente capacidades de administración de Windows Admin Center nativo HCI, incluida la información de hardware propietario en el panel y volver a agrupar la información de estado importante del clúster en una nueva interfaz fácil de usar.

Durante el proceso de desarrollo, se decidió implementar Windows Admin Center 1904 en una configuración de alta disponibilidad en el clúster, lo que garantiza la capacidad de administración aunque haya errores de nodo. La extensión viene preinstalada, al igual que el sistema operativo completo.

La extensión se creó en paralelo con Windows Admin Center 1904 desarrollan en Microsoft. Una cooperación estrecha y problemas de comentarios continuos expuestas en ambos lados que se resolvieron conjuntamente antes el producto que correctamente se lanzó en abril de 2019. Thomas Krenn es increíblemente orgulloso de ser uno de los primeros en admitir totalmente e implementar nuevas características del Windows Admin Center 1904.
