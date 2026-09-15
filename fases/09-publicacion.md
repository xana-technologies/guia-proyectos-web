# 09 · Publicación

**Qué es.** Poner la web en su dominio real y conectar lo que depende de él.
«Para publicar» y «Después de publicar» del *Protocolo de publicación
ESTÁNDAR*, adaptado a Astro + Vercel.

**Necesita antes.** 08.

## Para publicar

1. Dominio principal con o sin `www`.
2. DNS: registro A y CNAME (y TXT `_vercel` si el dominio estuvo en otra cuenta).
3. Comprobar certificado y redirección de la otra versión.

## Después de publicar

- `robots.txt` y `sitemap.xml` correctos → sitemap a Google Search Console.
- Formulario con correos definitivos → prueba real por formulario e idioma →
  confirmación del cliente.
- Tag Manager y Analytics → **informe tipo** (Looker Studio) → a las 24 h,
  comprobar que llegan datos.

## Quién

Belén o ingeniería con el DNS; Claude prepara los pasos y verifica. Nada se
publica sin Belén.

## Flujos

[Publicación](../flujos/publicacion.md)
