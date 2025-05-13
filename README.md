# Infraestructura_Ciudad

Enlace al repositorio: https://github.com/fferrlop/Infraestructura_Ciudad.git

Enlace a la página: https://fferrlop.github.io/Infraestructura_Ciudad/


![Estructura](https://github.com/user-attachments/assets/97087448-d739-4d52-9be9-c90b23118931)

# Explicación Breve del Proyecto

## Infraestructura General

La infraestructura de este proyecto ha sido diseñada para ser lo más segura posible dentro de las limitaciones de **Cisco Packet Tracer**.  
La estructura principal comprende tres edificios:

- Centro de Datos
- Ayuntamiento
- Casa de Inés

La conexión entre ellos se establece mediante **cables serial** utilizando **VPNs** para asegurar los paquetes, y un **túnel** para la zona externa (Casa de Inés).

---

## Conectividad IPv4 e IPv6

Entre el Centro de Datos y el Ayuntamiento:

![image](https://github.com/user-attachments/assets/e49ceeb9-5e6a-4da7-bc32-747055f64fcb)


- Todos los dispositivos funcionan correctamente con **IPv6**.
- Además, son capaces de comunicarse a través de **IPv4**.

![image](https://github.com/user-attachments/assets/48141252-165d-4bb8-bdee-fbd2a58cb0c0)


Se ha implementado **VPN site-to-site** para mayor seguridad, y **Dual Stack** para permitir la comunicación con casas particulares.

---

## Restricción de Accesos desde Casa Inés

Desde el Centro de Datos y el Ayuntamiento:

- Solo se permitirá la comunicación hacia la **Casa de Inés** desde los **servidores web**.
- No habrá acceso a ordenadores o archivos **FTP**.

![image](https://github.com/user-attachments/assets/a1c1a0d3-8296-4406-90a4-26da3036a316)


Esto garantiza una **mayor seguridad**.  
El ping desde la Casa de Inés hacia los edificios deberá hacerse mediante **IPv4**.  
Además, la conexión entre Casa Inés y Ayuntamiento será a través de una **VPN sobre un túnel GRE**.

![image](https://github.com/user-attachments/assets/00e8ee05-bdff-46fc-8c3c-ea88fdeefef4)

![image](https://github.com/user-attachments/assets/1095e5f1-94fa-4924-a080-f52814090e1e)

![image](https://github.com/user-attachments/assets/25b80455-e4f8-4f8d-be24-f35003d092ed)

---

## Implementación de ACLs

Para mejorar la lógica de la ciudad:

- Se han establecido **ACLs** para impedir el acceso a ordenadores o archivos desde casas particulares.
- Sin embargo, se permite acceder a los **servidores web** para consultar páginas oficiales.

![image](https://github.com/user-attachments/assets/07903415-f8bb-4c2b-98cb-ffa045ed9be9)

![image](https://github.com/user-attachments/assets/841288ae-fd49-4675-97ce-7628ca9ea31a)


Por ejemplo, si se hace un ping desde un ordenador particular hacia un dispositivo que no sea el servidor web, será denegado.

![image](https://github.com/user-attachments/assets/e2169ff9-8fd0-453d-ab5b-063054cae25a)


---

## Limitaciones de Cisco Packet Tracer

Durante el desarrollo se encontraron algunas limitaciones:

- **DNSSEC** no ha podido implementarse.
- **VPNs sobre IPv6** no han podido ser simuladas.

Por este motivo, **IPv6** se mantiene en un entorno seguro, sin exponerlo a zonas inseguras.

---

## Gestión de Archivos: FTP-TFTP

En los edificios de Centro de Datos y Ayuntamiento:

- Habrá servidores FTP-TFTP disponibles.
- Los dispositivos podrán guardar y compartir archivos en estos servidores.

![image](https://github.com/user-attachments/assets/eae5c38f-5789-44f7-bfb6-3a0f9c02b4cf)

Permitiendo así interactuar con el servidor desde dispositivos internos pudiendo crear y guardar documentos.

![image](https://github.com/user-attachments/assets/0ba16fc7-f1f8-4a24-8bf6-be6aea0b936a)

![image](https://github.com/user-attachments/assets/04ed2c94-c321-46d4-9490-4b0a17530b8f)

![image](https://github.com/user-attachments/assets/a1aba8a3-d296-4438-9187-ff68210949d6)

---

## Implementación de Dispositivos IoT

En la Casa de Inés se han implementado numerosos dispositivos IoT:

- **Sensores**
- **Cámaras**
- **Electrodomésticos**

Todo conectado a un **Home Gateway**.  
Además, se ha colocado una **Tablet** que controla todos los dispositivos mediante **IoT Monitor**, facilitando la gestión desde un único punto.

![image](https://github.com/user-attachments/assets/b48c670c-9a1b-4232-805c-6a003e83a8e4)

![image](https://github.com/user-attachments/assets/36fa9d97-c181-4e60-bb61-d764b9817fab)

![image](https://github.com/user-attachments/assets/477edeb9-82ee-422d-8f2d-79feac71e205)

---

## Interacción con SBCs

Algunos dispositivos IoT están conectados a **SBCs** para personalizar sus interacciones, como:

- **Sensor de Movimiento** → **Apertura automática de la puerta del garaje**.

![image](https://github.com/user-attachments/assets/42182889-714b-4ca7-a832-940e7ec1c5a0)


También se han implementado otras funciones:

- Semáforo inteligente

![image](https://github.com/user-attachments/assets/3054f230-0f26-4aeb-9765-fa19354f2f30)

- Alarma de incendios

![image](https://github.com/user-attachments/assets/538756aa-6387-4209-8089-a0e10d9b9ccf)

- Control de entrada

![image](https://github.com/user-attachments/assets/e56c022f-b673-439b-932f-a80728e37615)


Todo esto permite una simulación más realista de edificios y de una ciudad moderna.

---

