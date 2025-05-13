# Infraestructura_Ciudad

Enlace al repositorio: https://github.com/fferrlop/Infraestructura_Ciudad.git

Enlace a la página explicatoria: https://fferrlop.github.io/Infraestructura_Ciudad/


![Estructura](https://github.com/user-attachments/assets/97087448-d739-4d52-9be9-c90b23118931)

# Proyecto Integral de Redes - Paso 1: Diseño y Modelado de la Arquitectura

## Análisis de Modelos OSI y TCP/IP

La red del proyecto integra los modelos OSI y TCP/IP para lograr una infraestructura completa y funcional.

Modelo OSI:
- Capa Física: enlaces Ethernet, enlaces seriales, WiFi.
- Capa de Enlace: switches, VLANs, ARP.
- Capa de Red: direccionamiento IPv4 e IPv6, enrutamiento OSPFv2 y OSPFv3.
- Capa de Transporte: uso de TCP para servicios críticos (FTP, HTTP, DNS) y UDP para servicios en tiempo real (streaming, sensores IoT).
- Capas superiores: servicios DNS, FTP, HTTP/HTTPS, SSH.

Modelo TCP/IP:
- Capa de Acceso a Red: administración de conectividad física y lógica.
- Capa de Internet: IPv4/IPv6 y protocolos de enrutamiento.
- Capa de Transporte: TCP y UDP.
- Capa de Aplicación: servicios web, transferencia de archivos, resolución de nombres y control de dispositivos IoT.

La combinación de ambos modelos ha permitido ofrecer servicios seguros y eficientes para las áreas gubernamentales, de seguridad, transporte, multimedia e IoT.

### Diseño Lógico y Segmentación de la Red

La red se ha dividido lógicamente en áreas funcionales:

- Zona Gubernamental (Central2 - Ayuntamiento): administración pública, servidores y usuarios internos.
- Zona Centro de Datos (Central1): gestión de servidores públicos, web y FTP.
- Zona Residencial (Casa Inés): usuarios remotos conectados por VPN y entorno IoT doméstico.
- Zona de Emergencias y Seguridad: sensores de incendio, alarmas, cámaras de videovigilancia.
- Zona IoT y Multimedia (Casa Inés): dispositivos domóticos conectados a través de un Home Gateway.

Dispositivos utilizados:
- Routers Cisco 2811.
- Switches PT para acceso a fibra.
- Puntos de acceso inalámbrico.
- Servidores DNS, FTP, HTTP para la simulación de servicios municipales.
- PCs, laptops, cámaras IP, sensores de movimiento, sensores de temperatura, actuadores de luz y sonido.

Toda la estructura está interconectada utilizando enlaces seriales protegidos mediante VPN site-to-site (crypto-map en centrales y túnel GRE en Casa Inés), y optimizada mediante el protocolo de enrutamiento OSPF para IPv4 e IPv6.

# Proyecto Integral de Redes - Paso 2: Capa Física – Cálculos y Selección de Tecnologías

1. Cálculo de la Capacidad de los Enlaces

Para estimar la capacidad máxima de un enlace inalámbrico crítico se emplea la fórmula de Shannon:

C = B × log₂(1 + SNR)

Donde:
- C es la capacidad máxima de transmisión en bits por segundo.
- B es el ancho de banda del canal (en Hz).
- SNR es la relación señal-ruido en escala lineal.

Para un enlace crítico, se definieron los siguientes parámetros:
- Ancho de banda (B) = 300 MHz.
- Relación señal-ruido (SNR) = 20 dB.
- Conversión de SNR a escala lineal: SNR(lineal) = 10^(20/10) = 100.

Con estos datos se realizó la estimación de capacidad, lo cual permitió establecer la viabilidad de los enlaces inalámbricos propuestos.

2. Selección de Técnicas de Modulación

Para los enlaces cableados, se considera Ethernet estándar (modulación NRZ y PAM-5 en Gigabit Ethernet), ofreciendo alta fiabilidad y capacidad.

Para enlaces inalámbricos de sensores y dispositivos IoT, se analizaron diversas técnicas:
- BPSK: muy robusta pero limitada en capacidad.
- QPSK: buen compromiso entre eficiencia y robustez.
- 16-QAM: mayor eficiencia espectral, adecuada para ambientes controlados y enlaces de corto alcance.

Se optó por QPSK como la mejor solución para enlaces inalámbricos de sensores debido a su equilibrio entre tolerancia a interferencias y eficiencia de datos.

3. Evaluación de la Eficiencia del Encapsulamiento

En cada transmisión de datos se consideran las sobrecargas debidas a las cabeceras de protocolos en las capas de enlace y red.

Por ejemplo, para una trama Ethernet:
- Cabecera Ethernet: 18 bytes.
- Cabecera IPv4: 20 bytes.
- Cabecera TCP: 20 bytes.
Total de sobrecarga: 58 bytes por paquete.

Esto representa una pequeña fracción del tamaño total (típicamente 1500 bytes de MTU), pero se considera clave en enlaces inalámbricos y enlaces IoT de baja capacidad, donde la optimización de cada byte es crítica para el rendimiento global de la red.

# Proyecto Integral de Redes - Paso 3: Capa de Red – Direccionamiento, Subneteo y Enrutamiento

1. Diseño del Esquema de Direccionamiento IP

Se ha definido un esquema de direccionamiento estructurado y jerárquico para cada segmento:

- Centro de Datos (Central1):
  • LAN PCs: 192.168.10.0/24
  • Servidores: 192.168.20.0/24
  • IPv6 paralelo: 2001:DB8:1::/64 y 2001:DB8:3::/64

- Ayuntamiento (Central2):
  • LAN PCs: 192.168.30.0/24
  • Servidores: 192.168.40.0/24
  • IPv6 paralelo: 2001:DB8:11::/64 y 2001:DB8:13::/64

- Casa Inés:
  • LAN PCs: 192.168.10.0/24
  • IoT: 192.168.25.0/24
  • IPv6 paralelo: 2001:DB8:10::/64 y 2001:DB8:25::/64

Los enlaces WAN y VPN utilizan rangos /30 y /64 dedicados para una gestión eficiente.

Cada red /24 permite 254 hosts (rango de host 192.168.X.1 - 192.168.X.254). La estructura facilita la administración y crecimiento futuro.

2. Enrutamiento y Rutas Óptimas

Se ha implementado el protocolo OSPF (para IPv4) y OSPFv3 (para IPv6) para garantizar la convergencia rápida y óptima.

El algoritmo de Dijkstra se encarga de calcular las rutas más cortas basándose en la métrica de coste acumulado entre routers.

Topología de ejemplo:
- Central1 ↔ Central2 (enlace principal)
- Central2 ↔ Casa Inés (enlace VPN)

En caso de fallo de un enlace, la recuperación se basa en el re-cálculo automático de rutas por OSPF.

Adicionalmente, como técnica complementaria se contempla el enrutamiento por inundación (flooding) como método de respaldo extremo, aunque su uso en entornos reales está limitado debido a su alto consumo de ancho de banda y potencial para generar loops.

La solución de enrutamiento elegida en este proyecto asegura alta disponibilidad, eficiencia y posibilidad de expansión futura.

# Proyecto Integral de Redes - Paso 4: Capa de Transporte – Protocolos y Tamaño de Ventana

1. Selección de Protocolos de Transporte

Se han utilizado protocolos TCP y UDP de manera complementaria para cubrir todas las necesidades del proyecto.

TCP (Transmission Control Protocol):
- Se ha empleado para servicios que requieren alta fiabilidad y control de flujo:
  • Transferencia de archivos (FTP, SFTP)
  • Actualizaciones de bases de datos
  • Navegación web segura (HTTPS)

UDP (User Datagram Protocol):
- Se ha preferido para servicios que requieren baja latencia y tolerancia a pérdidas mínimas:
  • Streaming en tiempo real de cámaras de seguridad
  • Alertas de tráfico
  • Comunicaciones de sensores IoT y datos ambientales

2. Cálculo del Tamaño de Ventana en TCP

Para optimizar el rendimiento del protocolo TCP, se ha calculado el tamaño óptimo de la ventana utilizando la fórmula:

Ventana óptima = Ancho de banda × RTT

Con los siguientes parámetros:
- Ancho de banda estimado: 2 Gbps (2 × 10⁹ bits/segundo)
- RTT (Round Trip Time): 50 ms = 0.05 segundos

Cálculo:
Ventana óptima = 2 × 10⁹ bps × 0.05 s = 100,000,000 bits
= 12.5 Megabytes (1 byte = 8 bits)

Considerando un tamaño máximo de segmento TCP (MSS) de 1500 bytes:
12.5 MB / 1500 bytes ≈ 8333 segmentos simultáneos podrían estar en tránsito.

Este cálculo proporciona una referencia para el ajuste óptimo de las ventanas TCP en entornos de alta capacidad y baja latencia, como es el caso del backbone de la red del proyecto.

# Proyecto Integral de Redes - Paso 5: Capa de Aplicación – Servicios, Multiplexación y Multimedia

1. Implementación de Servicios y Resolución de Nombres

Se han implementado los siguientes servicios en la red:

- DNS (Domain Name System):
  • Se configuró un servidor DNS para la resolución interna de nombres y facilitar la conectividad dentro del entorno simulado.

- FTP/SFTP:
  • Se habilitó un servidor FTP para transferencia de archivos dentro de las sedes del Ayuntamiento y Centro de Datos.
  • En una red real, se emplearía SFTP para garantizar la seguridad de las transferencias.

- HTTP/HTTPS:
  • Se configuraron servidores web locales para simular la presencia institucional online.
  • HTTPS se utilizaría para asegurar la integridad y confidencialidad de las comunicaciones.

El proceso de resolución de nombres inicia cuando un usuario consulta una dirección simbólica; el servidor DNS traduce esta petición a una dirección IP. La multiplexación de servicios se logra mediante el uso de puertos diferentes para permitir múltiples sesiones simultáneas en un único dispositivo:
  • DNS → puerto 53
  • HTTP → puerto 80
  • HTTPS → puerto 443
  • FTP → puerto 21

2. Servicios Multimedia

Para la transmisión en tiempo real de vídeo de cámaras de seguridad y eventos públicos se ha planteado el uso de UDP Streaming.

UDP ofrece baja latencia y una mayor tolerancia a pérdidas menores de paquetes, siendo ideal para vídeo en tiempo real.

Dado que Cisco Packet Tracer no soporta DASH (Dynamic Adaptive Streaming over HTTP), se ha simulado la transmisión de vídeo bajo escenarios de ancho de banda variable.

En una red real, la calidad del contenido se adaptaría dinámicamente:
- Si el ancho de banda es alto → calidad HD.
- Si el ancho de banda es bajo → la resolución y la tasa de bits se reducen automáticamente para garantizar la continuidad del servicio.

Esto garantiza una experiencia fluida de usuario sin interrupciones graves incluso bajo condiciones de red degradada.

# Proyecto Integral de Redes - Paso 6: Seguridad – Estrategias y Configuración

1. Políticas y Medidas de Seguridad

Se ha diseñado un plan de seguridad para garantizar la protección de la infraestructura y los datos:

- VPN:
  • Se implementó una VPN site-to-site basada en IPsec con crypto-map entre Central1 (Centro de Datos) y Central2 (Ayuntamiento).
  • Se configuró una VPN sobre túnel GRE/IPsec entre Central2 y Casa Inés, asegurando el acceso remoto privado.

- Firewalls y ACLs:
  • En Central1 y Central2 se aplicaron ACLs para bloquear el acceso no autorizado.
  • Casa Inés tiene restricciones para solo permitir acceso a los servidores web institucionales.
  • Se bloquearon los accesos a otros recursos internos, aumentando la seguridad de la red.

2. Cifrado y Autenticación

TLS/SSL:
- Se ha previsto la implementación de TLS/SSL para proteger comunicaciones web sensibles en servicios HTTPS.

RSA (Rivest–Shamir–Adleman):
- En una red real se utilizaría para asegurar intercambios de claves.
- Ejemplo básico:
  • Un usuario genera un par de claves (privada y pública).
  • La clave pública se comparte, permitiendo a otros cifrar datos.
  • Solo el usuario con la clave privada puede descifrar la información.

Este esquema de cifrado garantiza la confidencialidad de los datos en entornos críticos.

3. Consideraciones sobre DNSSEC

DNSSEC proporciona autenticación y protección contra ataques de envenenamiento de caché (DNS Spoofing).

Sin embargo, Cisco Packet Tracer no soporta la configuración de DNSSEC en su entorno de simulación. Por ello, no se ha podido integrar esta funcionalidad en la red del proyecto.

En un entorno real, DNSSEC se aplicaría firmando digitalmente las respuestas DNS para validar su autenticidad y evitar manipulaciones maliciosas.

# Proyecto Integral de Redes - Paso 7: Implementación en Cisco Packet Tracer

1. Construcción de la Topología

La red completa se ha diseñado y configurado en Cisco Packet Tracer, simulando una infraestructura municipal completa:

- Dispositivos integrados:
  • Routers Cisco 2811 y 2901
  • Switches de acceso y distribución
  • Puntos de acceso inalámbrico (WiFi)
  • Servidores DNS, FTP, HTTP
  • PCs, laptops, cámaras de seguridad, sensores de temperatura, sensores de movimiento, actuadores y dispositivos IoT

La topología fue segmentada en zonas (Centro de Datos, Ayuntamiento, Casa Inés), asignando las direcciones IP y subredes conforme a la planificación establecida.

2. Configuración de Protocolos y Servicios

Se configuraron los siguientes elementos en Cisco Packet Tracer:

- Protocolos de enrutamiento:
  • OSPF (IPv4)
  • OSPFv3 (IPv6)
- VPNs:
  • Site-to-site IPsec VPN entre Central1 y Central2 (crypto-map)
  • VPN GRE/IPsec entre Central2 y Casa Inés
- Políticas de seguridad:
  • ACLs para limitar el acceso de Casa Inés únicamente a los servidores web de los edificios públicos
  • Firewall mediante listas de acceso
- Servicios configurados:
  • DNS para resolución de nombres
  • FTP para transferencia de archivos
  • HTTP para simulación de servicios web municipales
  • Streaming multimedia mediante UDP para cámaras de seguridad

3. Pruebas y Validación

Se realizaron exhaustivas pruebas de conectividad:

- Verificación de conectividad IPv4 e IPv6 entre todos los dispositivos.
- Comprobación de resolución de nombres mediante servidor DNS.
- Simulación de tráfico FTP y HTTP con éxito.
- Streaming funcional desde dispositivos de cámara en tiempo real.
- Validación de políticas de seguridad (VPN activas, ACLs aplicadas correctamente).

Evidencias en forma de capturas de pantalla fueron recopiladas para documentar el correcto funcionamiento y validar la entrega final del proyecto.
