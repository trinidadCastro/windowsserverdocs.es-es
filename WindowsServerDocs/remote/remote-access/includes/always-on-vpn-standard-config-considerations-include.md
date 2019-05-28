## <a name="standard-configuration-considerations"></a>Consideraciones sobre la configuración estándar

VPN de Always On tiene muchas opciones de configuración. Sin embargo, elija la configuración de VPN, sin embargo, incluya la siguiente información:

-   **Tipo de conexión.** Selección de protocolo de conexión es importante y finalmente sale codo con codo con el tipo de autenticación que se va a usar. Para obtener más información acerca de los protocolos de túnel disponibles, consulte [tipos de conexión VPN](https://docs.microsoft.com/windows/security/identity-protection/vpn/vpn-connection-type/).

-   **El enrutamiento.** En este contexto, las reglas de enrutamiento determinan si los usuarios pueden usar otras rutas de red mientras está conectado a la VPN.

    -   _Tunelización dividida_ permite el acceso simultáneo a otras redes, como Internet.

    -   _Túnel forzado_ requiere que todo el tráfico vaya exclusivamente a través de VPN y no permite el acceso simultáneo a otras redes.

-   **Desencadenar.** _Desencadenar_ determina cómo y cuándo se inicia una conexión VPN (por ejemplo, cuando se abre una aplicación, cuando el dispositivo está apagado, manualmente por el usuario). Para activar las opciones, consulte el [opciones de VPN desencadenada automáticamente perfil](https://docs.microsoft.com/windows/security/identity-protection/vpn/vpn-auto-trigger-profile/).

-   **Autenticación de dispositivo o usuario.** VPN de Always On utiliza certificados de dispositivo y las conexiones iniciadas por el dispositivo a través de una característica denominada [dispositivo túnel](https://docs.microsoft.com/windows-server/remote/remote-access/vpn/vpn-device-tunnel-config). Esa conexión se puede iniciar automáticamente y es persistente, similar de una conexión de túnel de infraestructura de DirectAccess.

>[!TIP]
>Al migrar de DirectAccess a VPN de Always On, tenga en cuenta a partir de las opciones de configuración que son comparables a lo que tiene y, a continuación, expanda desde allí.

Mediante el uso de certificados de usuario, el cliente de VPN de Always On se conecta automáticamente, pero lo hace en el nivel de usuario (después de inicio de sesión de usuario) en lugar de en el nivel de dispositivo (antes de inicio de sesión de usuario). La experiencia es todavía más fluida para el usuario, pero es compatible con los mecanismos de autenticación más avanzados, como Windows Hello para empresas.