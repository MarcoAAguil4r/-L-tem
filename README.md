# Proyecto de Laboratorio 22/09/2026
🍽️ L'tem SaaS

Plataforma SaaS Multi-Tenant para Gestión Integral de Restaurantes

L'tem (anteriormente Menú App) es una plataforma de software como servicio diseñada para la digitalización y operación de restaurantes. Permite a múltiples negocios operar simultáneamente de forma segura, con aislamiento total de datos (multi-tenancy) mediante PostgreSQL y Row Level Security (RLS).

Características Principales:

Menú Digital Interactivo (QR): Catálogos accesibles desde smartphones para los comensales.

Toma de Órdenes y Comandas: Registro en tiempo real desde la sala o directamente por el cliente.

Kitchen Display System (KDS): Panel de cocina en vivo con control de tiempos y estados de preparación.

Monitor de Sala y Planos: Mapeo interactivo de mesas por zonas y generador automático de QR.

Caja y Facturación: Control de cuentas, cobros y exportación de reportes de ventas en PDF.

Gestión de Reservas: Sistema de reservas online para clientes y panel administrativo para recepción.

Roles y Permisos (RBAC): Accesos granulares para Dueños, Administradores, Meseros, Cocineros y Recepcionistas.

Stack Tecnológico

Frontend:

React 19 + Vite (SPA)

React Router v7

Bootstrap 5 + CSS Custom Properties

Lucide React (Iconos) + Sonner (Notificaciones)

Backend & Base de Datos (Supabase):

PostgreSQL 15+ (Triggers, RPCs, Funciones)

Supabase Auth (GoTrue)

Supabase Realtime (WebSockets)

Supabase Storage

Infraestructura:

Docker & Docker Compose

Kubernetes (Kind) para despliegue local

-Inicio Rápido (Desarrollo Local)

Requisitos Previos

Node.js (v20 o superior)

Docker Desktop

Supabase CLI

Configuración

Clonar el repositorio e instalar dependencias:

npm install


Variables de Entorno:
Crea un archivo .env en la raíz del proyecto. Nunca incluyas la clave service_role aquí.

VITE_SUPABASE_URL=http://127.0.0.1:54421
VITE_SUPABASE_ANON_KEY=tu_clave_anon_local_de_supabase


Iniciar los servicios locales de Supabase (Docker):

npx supabase start


(El panel de Supabase Studio estará disponible en http://127.0.0.1:54423)

Ejecutar el servidor de desarrollo Vite:

npm run dev


(La aplicación estará disponible en http://localhost:5173)

-Despliegue con Kubernetes (Kind)

El proyecto incluye manifiestos para pruebas de orquestación local:

# 1. Crear clúster Kind
kind create cluster --config cluster.yaml --name menu-digital

# 2. Construir imagen Docker local
docker build -t menu-digital:latest -f dockerfile .

# 3. Cargar imagen al clúster
kind load docker-image menu-digital:latest --name menu-digital

# 4. Desplegar aplicación
kubectl apply -f deployment.yaml
kubectl apply -f service.yaml

# 5. Acceder en el navegador
# http://localhost:8080


-Arquitectura Multi-Tenant

El aislamiento de datos se gestiona directamente en la base de datos PostgreSQL utilizando Row Level Security (RLS). Cada tabla operativa contiene una columna company_id. Triggers automáticos y funciones (get_my_company_id()) aseguran que un usuario solo pueda leer, insertar o modificar datos correspondientes a su restaurante, eliminando la necesidad de gestionar el tenant manualmente desde el frontend.

-Documentación Completa

Para detalles exhaustivos sobre el modelo de datos, diagramas de secuencia, políticas de seguridad y la bitácora de vulnerabilidades corregidas, por favor consulta el archivo DOCUMENTACION.md incluido en este repositorio.
