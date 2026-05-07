# Docker WebServers CV — Apache & Nginx con HTTPS/HTTP2

**Materia:** Programación Web  
**Proyecto:** Despliegue de CVs con servidores web en contenedores Docker

---

## Descripción

Este proyecto consiste en levantar dos servidores web (**Apache** y **Nginx**) usando **Docker**, cada uno sirviendo tres CVs personales (`cv1`, `cv2`, `cv3`) desarrollados con HTML y CSS. Además se configuró **HTTPS con certificado autofirmado** y soporte para **HTTP/2**.

---

## Estructura del proyecto

```
proyecto-dock/
├── apache/
│   └── httpd.conf          # Configuración de Apache con SSL y HTTP/2
├── certs/
│   ├── cert.pem            # Certificado SSL autofirmado
│   └── key.pem             # Clave privada SSL
├── cvs/
│   ├── cv1/
│   │   ├── index.html      # CV 1 - Mobile First
│   │   ├── styles.css
│   │   └── img/
│   ├── cv2/
│   │   ├── index.html      # CV 2 - Kopeina
│   │   ├── styles.css
│   │   └── img/
│   └── cv3/
│       ├── index.html      # CV 3 - W3CSS
│       └── img/
├── nginx/
│   └── nginx.conf          # Configuración de Nginx con SSL y HTTP/2
└── docker-compose.yml      # Orquestación de contenedores
```

---

## Tecnologías utilizadas

| Tecnología | Uso |
|------------|-----|
| Docker & Docker Compose | Contenedorización de servidores |
| Apache HTTP Server 2.4 | Servidor web 1 |
| Nginx | Servidor web 2 |
| HTML5 + CSS3 | Desarrollo de los CVs |
| OpenSSL | Generación de certificados SSL |
| HTTPS / HTTP2 | Protocolo seguro de transferencia |

---

## Instrucciones de uso

### 1. Generar los certificados SSL

```bash
mkdir certs
openssl req -x509 -nodes -days 365 -newkey rsa:2048 \
  -keyout certs/key.pem \
  -out certs/cert.pem \
  -subj "/CN=localhost"
```

### 2. Levantar los contenedores

```bash
docker compose up -d
```

### 3. Verificar que estén corriendo

```bash
docker compose ps
```

---

## URLs de acceso

### HTTP (sin SSL)
| URL | Servidor |
|-----|----------|
| `http://localhost:8080/cv1` | Apache |
| `http://localhost:8080/cv2` | Apache |
| `http://localhost:8080/cv3` | Apache |
| `http://localhost:8081/cv1` | Nginx |
| `http://localhost:8081/cv2` | Nginx |
| `http://localhost:8081/cv3` | Nginx |

### HTTPS + HTTP/2
| URL | Servidor |
|-----|----------|
| `https://localhost:8443/cv1` | Apache |
| `https://localhost:8443/cv2` | Apache |
| `https://localhost:8443/cv3` | Apache |
| `https://localhost:8444/cv1` | Nginx |
| `https://localhost:8444/cv2` | Nginx |
| `https://localhost:8444/cv3` | Nginx |

> Al abrir las URLs con HTTPS el navegador mostrará una advertencia de seguridad porque el certificado es autofirmado. Haz clic en **Opciones avanzadas → Continuar de todas formas**.

---

## Configuración de los servidores

### Nginx (`nginx/nginx.conf`)
- Redirige HTTP → HTTPS automáticamente
- Habilita HTTP/2 con `http2 on`
- Sirve los 3 CVs desde `/usr/share/nginx/html`

### Apache (`apache/httpd.conf`)
- Carga módulos SSL y HTTP/2
- Configura VirtualHost en puerto 443
- Habilita `Protocols h2 http/1.1`

---

## Conceptos aprendidos

- **Docker Compose:** múltiples contenedores
- **Virtual Hosts:** servir múltiples sitios desde un mismo servidor
- **HTTPS/TLS:** cifrado de comunicación con certificados SSL
- **HTTP/2:** protocolo más eficiente que HTTP/1.1 (multiplexación, compresión de cabeceras)
- **Apache vs Nginx:** diferencias de configuración entre ambos servidores
