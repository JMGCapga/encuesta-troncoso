# Encuesta de Satisfacción - Materiales Troncoso

Página de encuesta digital para recopilar feedback de clientes después de la entrega.

## Características

- ✅ 5 preguntas de satisfacción (Sí/No)
- 📝 Campo de comentarios opcional
- 📱 Optimizado para móvil (botones grandes para usar con guantes)
- 🎁 Incentivo: playera de regalo por reseña en Google Maps
- 🔗 Sin login requerido — acceso por link directo
- 🔥 Integración con Firebase Firestore
- 🔌 **Modo embedded** — mostrar como modal en el CRM después de firmar remisión

## Modos de uso

### 1. Modo Standalone (página web pública)

Link con parámetros:
```
https://jmgcapga.github.io/encuesta-troncoso?pedido=TRO-045&cliente=Juan%20García
```

Parámetros:
- `pedido`: ID del pedido (ej. TRO-045)
- `cliente`: Nombre del cliente

### 2. Modo Embedded (en el CRM después de firmar remisión)

**Opción A: Link en iframe con parámetro embedded**
```html
<iframe src="https://jmgcapga.github.io/encuesta-troncoso?pedido=TRO-045&cliente=Juan&embedded=true" 
        style="width:100%; height:100vh; border:none;"></iframe>
```

**Opción B: Llamada JavaScript directa (recomendada)**

Agregar este script en el CRM (después de cargar index.html):

```javascript
// Cargar iframe con encuesta
const encuestaFrame = document.createElement('iframe');
encuestaFrame.src = 'https://jmgcapga.github.io/encuesta-troncoso';
encuestaFrame.style.cssText = 'position:fixed; top:0; left:0; width:100%; height:100%; border:none; z-index:9999;';
document.body.appendChild(encuestaFrame);

// Esperar a que cargue y llamar a la función
encuestaFrame.onload = function() {
    encuestaFrame.contentWindow.mostrarEncuesta({
        pedidoId: 'TRO-045',
        clienteNombre: 'Juan García',
        onCompleta: function(datos) {
            console.log('Encuesta completada:', datos);
            // Aquí cerrar el iframe o hacer algo más
            encuestaFrame.style.display = 'none';
        }
    });
};
```

**API de funciones (modo embedded):**

```javascript
// Mostrar encuesta como modal
window.mostrarEncuesta({
    pedidoId: 'TRO-045',          // ID del pedido
    clienteNombre: 'Juan García', // Nombre del cliente
    onCompleta: callback          // Callback cuando se envía (opcional)
});

// Cerrar encuesta
window.cerrarEncuesta();
```

## Integración con CRM para envío a 2 números

Cuando se confirma la entrega en el CRM, enviar WhatsApp a **DOS números si están disponibles**:

```javascript
function enviarEncuestasADosNumeros(pedido, cotizacion) {
    const numeros = [];
    
    // Número 1: Cliente en pedido (quien pagó)
    if (pedido.telefono) {
        numeros.push(pedido.telefono);
    }
    
    // Número 2: Cliente en cotización (si es diferente)
    if (cotizacion.telefono && cotizacion.telefono !== pedido.telefono) {
        numeros.push(cotizacion.telefono);
    }
    
    // Enviar a cada número
    numeros.forEach(function(telefono) {
        const linkEncuesta = 'jmgcapga.github.io/encuesta-troncoso?pedido=' + 
            encodeURIComponent(pedido.id) + '&cliente=' + 
            encodeURIComponent(pedido.cliente);
        
        const mensaje = '📦 *MATERIALES TRONCOSO*\n' +
            '¡Su pedido ' + pedido.id + ' fue entregado! ¡Gracias por su preferencia!\n\n' +
            '⭐ ¿Nos regala 30 segundos?\n' +
            'Su opinión nos ayuda a mejorar el servicio:\n' +
            '👉 ' + linkEncuesta + '\n\n' +
            '🎁 Si quedó satisfecho y nos deja una reseña en Google,\n' +
            '¡le regalamos una playera Materiales Troncoso\n' +
            'en su próxima visita!\n\n' +
            '📞 5555906802\n' +
            'materialestroncoso.com';
        
        // Abrir WhatsApp con mensaje
        window.open('https://wa.me/' + telefono + '?text=' + encodeURIComponent(mensaje));
    });
}
```

## Diseño

- Color naranja: `#E8500A` (Materiales Troncoso)
- Botones SÍ: Verde `#22C55E`
- Botones NO: Gris `#F0EDE8`
- Fuentes: Bebas Neue (títulos), Barlow (texto)
- Mobile-first responsive
- Versión embedded: más compacta y adaptada a pantalla de operador

## Firebase

- Proyecto: `troncoso--sistema`
- Colección: `encuestas`
- Estructura de documento:
  ```json
  {
    "pedidoId": "TRO-045",
    "cliente": "Juan García",
    "fecha": "2026-07-13",
    "timestamp": 1689270000000,
    "llegóATiempo": true,
    "productosCorrectos": true,
    "cantidadesCorrectas": true,
    "descargaSinDanos": true,
    "atencionBuena": true,
    "comentarios": "Todo excelente",
    "puntuacion": 5,
    "dejóResena": false
  }
  ```

## Deploy

1. Crear repo en GitHub: `JMGCapga/encuesta-troncoso`
2. Push a main
3. Activar GitHub Pages en Settings → Pages → main
4. La página estará disponible en: `https://jmgcapga.github.io/encuesta-troncoso`

## Flujo de uso en CRM

1. **Operador entrega material** → Cliente firma remisión digital
2. **Al confirmar entrega** → Mostrar encuesta en modal en teléfono del operador
3. **Cliente llena encuesta en el mismo teléfono** → Se guarda en Firebase
4. **Automáticamente** → CRM envía WhatsApp con link a encuesta a:
   - Teléfono del cliente (quien pagó)
   - Teléfono de la persona que recibió (si es diferente) — del campo `telefono_recibe` en cotización

---
Materiales Troncoso © 2026
