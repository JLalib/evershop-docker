# Cómo instalar Evershop en Docker - Plataforma ecommerce moderna en Docker

## EverShop: Plataforma ecommerce moderna TypeScript+React en Docker

Tienda online profesional en minutos. GraphQL API, React frontend, modular, customizable. Headless listo. Admin dashboard completo. Open source GPL-3.0. Arranca en 5 comandos.

## ¿Qué es EverShop?

EverShop es una plataforma ecommerce moderna construida con TypeScript, React y GraphQL que permite crear tiendas online profesionales sin configuración compleja. A diferencia de Shopify (SaaS pago) o Magento (complejo), EverShop es open source, minimalista pero completo, y se instala en minutos con Docker.

Stack moderno: Backend en Node.js con Express, API GraphQL, frontend React con SSR, PostgreSQL para datos, arquitectura modular y extensible. Panel de administración profesional, gestión de productos, órdenes, clientes, pagos, inventario, y listo para ser headless (API para mobile apps, PWAs, etc).

Para developers: Si quieres tu propia tienda online profesional pero bajo control total (no SaaS), o necesitas construir ecommerce personalizado para clientes, EverShop es tu herramienta. Completamente customizable, modular, listo para producción.

## Características principales

### GraphQL API
API moderna, eficiente, perfecta para apps mobile y PWAs.

### Admin dashboard
Panel profesional para gestionar tienda, productos, órdenes.

### React frontend
Tienda web moderna, rápida, con SSR optimizado.

### Gestión de productos
Categorías, atributos, variantes, imágenes, SEO.

### Carrito y checkout
Experiencia de compra completa, profesional.

### Órdenes y clientes
Gestión completa de pedidos, historial, cuentas usuario.

### Pagos integrados
Stripe, PayPal, integraciones de pago populares.

### Inventario
Control de stock, alertas, gestión de inventario.

### Modular y extensible
Sistema de plugins, custom extensions, sin tocar core.

### Headless ready
API para mobile apps, PWAs, integraciones externas.

### Temas customizables
Crear temas propios, cambiar look&feel sin código.

### Open Source
GPL-3.0. Control total, sin vendor lock-in.

## Requisitos del sistema

- Docker & Docker Compose
- 4-8 GB RAM mínimo (Node.js + React requiere recursos)
- 2+ CPU cores
- 20+ GB espacio disco (base de datos, imágenes)
- PostgreSQL 12+ (incluido en docker-compose)
- Puertos: 3000 (frontend), 8080 (API), 5432 (DB)

Nota: EverShop es más pesado que Dufs pero más ligero que Magento. Node.js + React consumen recursos. Mínimo 2GB RAM recomendado.

## Instalación con Docker Compose

### Opción 1: Instalación rápida (30 segundos)

```bash
curl -sSL https://raw.githubusercontent.com/evershopcommerce/evershop/main/docker-compose.yml > docker-compose.yml
docker compose up -d
```

### Opción 2: Personalizado

```yaml
version: '3.8'

services:
  postgres:
    image: postgres:15
    container_name: evershop_db
    restart: unless-stopped
    environment:
      - POSTGRES_DB=evershop
      - POSTGRES_USER=evershop
      - POSTGRES_PASSWORD=tu-contraseña-segura
    volumes:
      - postgres_data:/var/lib/postgresql/data

  evershop:
    image: evershop/evershop:latest
    container_name: evershop
    restart: unless-stopped
    ports:
      - "3000:3000"
      - "8080:8080"
    environment:
      - DB_HOST=postgres
      - DB_PORT=5432
      - DB_NAME=evershop
      - DB_USER=evershop
      - DB_PASSWORD=tu-contraseña-segura
    depends_on:
      - postgres

volumes:
  postgres_data:
```

### Iniciar

```bash
docker compose up -d
```

### Acceder

- Tienda web: http://localhost:3000
- Admin: http://localhost:3000/admin (credenciales por email después del setup)
- API GraphQL: http://localhost:8080/graphql

## Primeros pasos

### 1. Setup inicial

- Accede a http://localhost:3000
- Se abre wizard de setup (crear admin, tienda)
- Completa información de tienda: nombre, email, contraseña
- Se crea BD automáticamente y te loguea

### 2. Acceder al admin

- Ve a http://localhost:3000/admin
- Login con credenciales que creaste
- Dashboard con estadísticas, últimas órdenes, productos

### 3. Agregar productos

- Admin → Products → New Product
- Nombre, descripción, precio, imágenes, atributos
- SKU, inventario, categorías
- Save

### 4. Configurar pago

- Admin → Settings → Payment
- Stripe: Agrega API keys
- PayPal: Agrega credenciales

### 5. Ver tienda

- http://localhost:3000
- Ves productos que agregaste
- Prueba carrito, checkout (con pago de prueba)

## Casos de uso

- Tienda online personal: Tu propia tienda sin pagar Shopify
- Agencia web: Construir ecommerce para clientes
- Mvp de startup: Validar modelo de negocio rápidamente
- Marketplace: Base para expandir a multi-vendor
- Headless para apps: API para mobile apps, PWAs
- Integración con sistemas: ERP, CRM, automatizaciones

## Gestión y mantenimiento

### Ver logs

```bash
docker logs -f evershop
```

### Reiniciar

```bash
docker compose restart
```

### Actualizar

```bash
docker compose pull
docker compose up -d
```

### Backup de base de datos

```bash
docker exec evershop_db pg_dump -U evershop evershop > backup-$(date +%Y%m%d).sql
```

### Restore de backup

```bash
cat backup-20260522.sql | docker exec -i evershop_db psql -U evershop evershop
```

### Acceso a base de datos

```bash
docker exec -it evershop_db psql -U evershop evershop
```

## HTTPS con Caddy

### Configurar para producción

```
tienda.tudominio.com {
  reverse_proxy localhost:3000
}

admin.tudominio.com {
  reverse_proxy localhost:3000/admin
}

api.tudominio.com {
  reverse_proxy localhost:8080
}
```

### Variables de entorno para URL correcta

```yaml
environment:
  - STOREFRONT_URL=https://tienda.tudominio.com
  - ADMIN_URL=https://admin.tudominio.com
  - API_URL=https://api.tudominio.com
```

## Redes

- 📼 Youtube: [https://www.youtube.com/@genbyte](https://www.youtube.com/@genbyte)
- ⛓ Github: [https://github.com/JLalib](https://github.com/JLalib)
- 💻 Blog: [https://genbyte.blogspot.com/](https://genbyte.blogspot.com/)
- 🐦 X (Twitter): [https://twitter.com/gen_byte](https://twitter.com/gen_byte)

