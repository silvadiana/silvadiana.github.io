---
title: TAREA - BUFFER OVERFLOW
date: 2024-11-04 00:00:00 -05:00
categories: [Vulnerabilities, Windows 2008, Metasploit]
tags: [bufferoverflow]
---

# Tarea.

En clase, vimos como obtener las direcciones de retorno de la instrucción JMP ESP, la cual es la instrucción que le indica al runtime que debe retornar a la memoria de stack pointer.

**!mona find -s "\xff\xe4" -m essfunc.dll**

## Requerimientos.
1. El objetivo principal de esta tarea consiste en revisar si cada una de esas direcciones restantes es vulnerable y permite realizar el buffer overflow que, por consecuencia, permita ejecutar el exploit y obtener una shell por conexión tcp reversa.

- Obtención de Direcciones de Retorno
    - Uso de Mona:
        Ejecutar el comando !mona find -s "\xff\xe4" -m essfunc.dll en el entorno de depuración (por ejemplo, Immunity Debugger) para identificar las direcciones de retorno que pueden ser utilizadas en el exploit.

        ![figura](/assets/images/2024-10-25-clase07-s08/Captura de pantalla 2024-11-05 122240.png)

- Revisión de Vulnerabilidades:

    - Para cada dirección de retorno obtenida, se debe realizar un análisis para verificar si permite un buffer overflow. Esto puede incluir:
        - Inyección de Payloads: Probar cada dirección con un payload diseñado para causar un buffer overflow.
        - Monitoreo de Comportamiento: Utilizar herramientas como Wireshark o el propio depurador para observar si se puede ejecutar código arbitrario.


2. Como segundo objetivo, el alumno debe de explicar en su procedimiento ¿qué es una conexión tcp reversa? Utilizar un gráfico relacionado a su setup de laboratorio (VM Kali vs VM Windows, VM Kali vs Host Windows) y desarrollar la explicación correspondiente.

    - Explicación: Una conexión TCP reversa es un método en el cual una máquina comprometida (target) inicia una conexión a un atacante (attacker) en lugar de que el atacante se conecte a la máquina comprometida. Esto es útil, especialmente en redes donde el tráfico de entrada está restringido.
    Gráfico del Setup de Laboratorio:

    - VM Kali (Atacante):
        Escucha en un puerto específico (por ejemplo, 4444).

    - VM Windows (Objetivo):
        Ejecuta el exploit que, al ser exitoso, hace que la máquina Windows se conecte de vuelta a Kali.

    - Explicación del Procedimiento:
        Al ejecutar el exploit en la máquina objetivo, este provoca que la máquina se conecte al puerto 4444 en la VM Kali. Una vez establecida la conexión, se puede obtener una shell de comandos que permite ejecutar comandos en la máquina objetivo.

3. Una vez que obtiene la conexión tcp reversa (suficiente con utilizar una de las direcciones de retorno que permite una exitosa ejecución del exploit), investigar los permisos de la shell (es decir, del usuario que ejecuta aquella shell).
- Obtener Permisos: 
        - Una vez que se obtiene la shell, se puede verificar el usuario y los permisos ejecutando el comando whoami o id en la shell obtenida.
        - Esto permitirá determinar si la shell tiene privilegios de administrador o si está limitada a un usuario normal.

        ```bash
            whoami
        ```