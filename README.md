<!DOCTYPE html>
<html lang="es">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1">
  <title>Galería de Productos</title>
  <style>
    /* Estilos Generales */
    body {
      font-family: Arial, sans-serif;
      margin: 0;
      padding: 0;
      background-color: #f5f5f5;
      color: #333;
    }

    header {
      background-color: #007BFF;
      padding: 20px;
      text-align: center;
      color: white;
    }

    main {
      padding: 20px;
      max-width: 1200px;
      margin: auto;
    }
    
    /* Galería de Productos */
    .gallery {
      display: grid;
      grid-template-columns: repeat(auto-fill, minmax(250px, 1fr));
      gap: 20px;
    }

    .product {
      background: white;
      border-radius: 5px;
      box-shadow: 0 2px 5px rgba(0,0,0,0.1);
      overflow: hidden;
      transition: transform 0.2s;
    }

    .product:hover {
      transform: scale(1.02);
    }

    .product img {
      width: 100%;
      display: block;
    }

    .product .info {
      padding: 15px;
    }

    .product .info h3 {
      margin: 0 0 10px;
      font-size: 1.1em;
    }

    /* Botón de Me Gusta */
    .like-btn {
      background-color: #e0e0e0;
      border: none;
      padding: 8px 12px;
      border-radius: 4px;
      cursor: pointer;
      font-size: 0.9em;
      transition: background-color 0.3s;
    }

    .like-btn:hover {
      background-color: #ccc;
    }

    /* Estado activo del like */
    .like-btn.liked {
      background-color: #ff4081;
      color: white;
    }

    .like-btn .like-count {
      font-weight: bold;
      margin-left: 5px;
    }

    /* Botón para cargar más productos */
    #loadMore {
      display: block;
      margin: 30px auto 0;
      padding: 10px 20px;
      font-size: 1em;
      background-color: #007BFF;
      color: white;
      border: none;
      border-radius: 5px;
      cursor: pointer;
      transition: background-color 0.3s;
    }

    #loadMore:hover {
      background-color: #0056b3;
    }
  </style>
</head>
<body>
  <header>
    <h1>Galería de Productos</h1>
  </header>
  <main>
    <!-- Contenedor de la Galería -->
    <div class="gallery" id="gallery">
      <!-- Producto 1 -->
      <div class="product">
        <img src="https://via.placeholder.com/300x200?text=Producto+1" alt="Producto 1">
        <div class="info">
          <h3>Producto 1</h3>
          <button class="like-btn" data-liked="false">
            Me gusta <span class="like-count">0</span>
          </button>
        </div>
      </div>
      <!-- Producto 2 -->
      <div class="product">
        <img src="https://via.placeholder.com/300x200?text=Producto+2" alt="Producto 2">
        <div class="info">
          <h3>Producto 2</h3>
          <button class="like-btn" data-liked="false">
            Me gusta <span class="like-count">0</span>
          </button>
        </div>
      </div>
      <!-- Puedes agregar más productos aquí inicialmente -->
    </div>
    <!-- Botón para cargar más productos -->
    <button id="loadMore">Mostrar más productos</button>
  </main>

  <script>
    // Funcionalidad de "Me gusta"
    document.querySelectorAll('.like-btn').forEach(btn => {
      btn.addEventListener('click', function() {
        // Verificar estado del botón (ya marcado o no)
        const isLiked = this.getAttribute('data-liked') === 'true';
        const countSpan = this.querySelector('.like-count');
        let count = parseInt(countSpan.textContent);

        if (!isLiked) {
          count += 1;
          this.setAttribute('data-liked', 'true');
          this.classList.add('liked');
        } else {
          count -= 1;
          this.setAttribute('data-liked', 'false');
          this.classList.remove('liked');
        }
        countSpan.textContent = count;
      });
    });

    // Funcionalidad para "Mostrar más productos"
    const loadMoreButton = document.getElementById('loadMore');
    const gallery = document.getElementById('gallery');
    // Array con nuevos productos (puedes obtener esta data de una API o base de datos)
    const nuevosProductos = [
      {
        id: 3,
        titulo: "Producto 3",
        imagen: "https://via.placeholder.com/300x200?text=Producto+3"
      },
      {
        id: 4,
        titulo: "Producto 4",
        imagen: "https://via.placeholder.com/300x200?text=Producto+4"
      },
      {
        id: 5,
        titulo: "Producto 5",
        imagen: "https://via.placeholder.com/300x200?text=Producto+5"
      },
      {
        id: 6,
        titulo: "Producto 6",
        imagen: "https://via.placeholder.com/300x200?text=Producto+6"
      }
    ];
    let productosMostrados = 0;
    const productosPorCarga = 2; // Número de productos a agregar por clic

    loadMoreButton.addEventListener('click', () => {
      // Determinar el rango de productos a cargar
      const inicio = productosMostrados;
      const fin = productosMostrados + productosPorCarga;
      const productosACargar = nuevosProductos.slice(inicio, fin);

      productosACargar.forEach(producto => {
        // Crear el elemento HTML para cada producto
        const productDiv = document.createElement('div');
        productDiv.classList.add('product');
        productDiv.innerHTML = `
          <img src="${producto.imagen}" alt="${producto.titulo}">
          <div class="info">
            <h3>${producto.titulo}</h3>
            <button class="like-btn" data-liked="false">
              Me gusta <span class="like-count">0</span>
            </button>
          </div>
        `;
        gallery.appendChild(productDiv);

        // Agregar la funcionalidad de "me gusta" al nuevo botón
        const likeButton = productDiv.querySelector('.like-btn');
        likeButton.addEventListener('click', function() {
          const isLiked = this.getAttribute('data-liked') === 'true';
          const countSpan = this.querySelector('.like-count');
          let count = parseInt(countSpan.textContent);

          if (!isLiked) {
            count += 1;
            this.setAttribute('data-liked', 'true');
            this.classList.add('liked');
          } else {
            count -= 1;
            this.setAttribute('data-liked', 'false');
            this.classList.remove('liked');
          }
          countSpan.textContent = count;
        });
      });

      productosMostrados += productosPorCarga;
      // Si no hay más productos por cargar, se oculta el botón
      if (productosMostrados >= nuevosProductos.length) {
        loadMoreButton.style.display = 'none';
      }
    });
  </script>
</body>
</html>
