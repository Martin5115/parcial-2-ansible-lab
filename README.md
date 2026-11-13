# Práctica 2do Parcial - Ansible

ITLA 
Nombre	Martin Gomez
Matrícula	2024-2481
Profesor	Elvys Cruz
Fecha	12/11/2026
Tema	Segundo Parcial - Electiva 2

## Objetivo del parcial

Crear un laboratorio con dos servidores Linux (Ubuntu 24.04) utilizando Docker y
administrarlos con Ansible mediante un inventario y un playbook que muestra
información del host, crea directorios y archivos, usa un `loop` y una
condición (`when`).

## Estructura del proyecto

```
ansible-lab/
├── docker-compose.yml
├── inventory.ini
├── playbook.yml
├── README.md
└── captura.png
```

## Requisitos

- Docker y Docker Compose
- Ansible (`sudo apt install ansible`)
- En Windows, ejecutar Ansible desde WSL2 (Ubuntu) con Docker Desktop integrado

## Comandos para iniciar los contenedores

```bash
docker compose up -d
docker ps
```

Instalar Python en cada contenedor (requisito de Ansible):

```bash
docker exec server1 bash -c "apt update && apt install -y python3"
docker exec server2 bash -c "apt update && apt install -y python3"
```

Comprobar la conexión con el inventario:

```bash
ansible docker -i inventory.ini -m ping
```

## Comando para ejecutar el playbook

```bash
ansible-playbook -i inventory.ini playbook.yml
```

## Qué hace el playbook

En `server1` y `server2`:

1. Muestra el nombre del host.
2. Muestra el sistema operativo.
3. Crea el directorio `/tmp/ansible-demo`.
4. Crea el archivo `info.txt` con el nombre del servidor y el sistema operativo.
5. Crea con un `loop` los directorios `logs`, `backup` y `config`.
6. Muestra un mensaje solo cuando el sistema operativo es Ubuntu (`when`).

## Verificación

```bash
docker exec server1 cat /tmp/ansible-demo/info.txt
docker exec server1 ls /tmp/ansible-demo
```

Salida esperada de `info.txt`:

```
Servidor: server1
Sistema operativo: Ubuntu 24.04
```

## Captura de la ejecución exitosa

Resultado final del playbook (`PLAY RECAP` con `failed=0` en ambos servidores) y verificación del archivo `info.txt`:

<img width="1600" height="860" alt="captura2" src="https://github.com/user-attachments/assets/722c38d3-c8e3-4aa0-b693-45b05130bff6" />

Prueba de conexión (`ping`) e inicio de la ejecución:

<img width="1586" height="783" alt="captura" src="https://github.com/user-attachments/assets/56c6b54b-767a-4041-9825-367e340016db" />

## Detener el laboratorio

```bash
docker compose down
```
