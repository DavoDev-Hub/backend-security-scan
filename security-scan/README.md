# Análisis de seguridad con Nuclei — backend

## Cómo correrlo

1. Levanta el stack (si no está corriendo):
   ```sh
   docker compose up -d --build
   ```
2. Corre el escaneo:
   ```sh
   ./security-scan/run-nuclei.sh
   ```
   (por defecto apunta al servicio `backend`, puerto 5000 — Flask)

El contenedor de Nuclei se conecta con `--network container:<id del contenedor>`,
compartiendo el namespace de red del contenedor `backend` directamente. Así
no depende de que el puerto esté publicado al host ni de adivinar el nombre
de la red de `docker-compose` — es el "bridge" entre Nuclei y lo que se está
analizando.

3. El resultado queda en `security-scan/results/nuclei-backend.txt`.

## Flujo de entrega

1. Correr el escaneo y guardar el resultado en esta carpeta.
2. `git add security-scan/ && git commit -m "Agrega resultados de análisis Nuclei" && git push`.
3. Revisar cada hallazgo:
   - Si es de complejidad alta y el profesor indica que no se debe
     implementar, se deja documentado aquí (con la razón) y no se toca.
   - El resto se corrige **uno por uno**, con un commit independiente por
     cada corrección, para que el historial del repo muestre claramente
     qué se arregló y por qué.

## Notas

- La mayoría de los endpoints de este backend requieren `Authorization:
  Bearer <jwt>`, así que Nuclei probablemente solo pueda enumerar cosas
  visibles sin autenticación (headers de seguridad, `/health`, manejo de
  errores, etc.) — eso también es información útil para endurecer el
  servicio.
- Si el escaneo no encuentra nada, igual se documenta aquí como evidencia
  de que se corrió.
