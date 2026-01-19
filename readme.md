 
# pg_progress 📊

**pg_progress** es una funcion ligera  para PostgreSQL escrita en PL/pgSQL que permite renderizar barras de progreso dinámicas, coloridas y personalizables directamente en la consola de `psql`.

Ideal para DBAs y Desarrolladores que necesitan monitorear procesos de larga duración como migraciones de datos, tareas ETL o mantenimiento de tablas sin salir de la terminal.



---

## ✨ Características

* **100% Nativo:** Escrito en PL/pgSQL, sin dependencias externas.
* **Colores Dinámicos:** Los colores cambian según el porcentaje (Rojo, Amarillo, Azul y Verde) usando códigos ANSI.
* **Estilos Personalizables:** Incluye 3 tipos de barras predefinidas (Clásica, Bloques y Flechas).
* **Ligero y Eficiente:** Diseñado para actualizar la misma línea en `psql`, evitando llenar la consola de mensajes repetitivos.

## 🚀 Instalación

Para instalar la función en tu base de datos, simplemente ejecuta el script SQL:

```bash
psql -d tu_base_de_datos -f sql/pg_progress.sql

```

## 🛠️ Cómo usarlo

### Uso básico

La función devuelve un `TEXT`, por lo que puedes usarla en un `SELECT`:

```sql
SELECT pg_render_progress_bar(75, 1);

```

### Tipos de barras

Puedes elegir entre tres estilos pasando un segundo parámetro (1, 2 o 3):

1. **Clásica:** `[██████████░░░] 75%`
2. **Bloques:** `|■■■■■■■■■■□□□| 75%`
3. **Flechas:** `《►►►►►►►►►►···》 75%`

### Ejemplo en un script real

Para que la barra se actualice en la misma línea y no haga scroll, usa secuencias de escape en un bloque `DO` o función:

```sql
DO $$
DECLARE
    i INTEGER;
BEGIN
    FOR i IN 0..100 LOOP
        -- \033[1G mueve el cursor al inicio de la línea
        RAISE NOTICE E'\033[1G%', pg_render_progress_bar(i, 1);
        
        PERFORM pg_sleep(0.05); -- Simula trabajo
    END LOOP;
END $$;

```


## 🎨 Lógica de Colores

| Porcentaje | Color |
| --- | --- |
| 0% - 24% | 🔴 Rojo |
| 25% - 49% | 🟡 Amarillo |
| 50% - 74% | 🔵 Azul |
| 75% - 100% | 🟢 Verde |
