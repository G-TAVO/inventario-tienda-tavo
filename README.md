
  <h1>Tienda Emanuel</h1>

  <!-- Inventario -->
  <section>
    <h2>Inventario</h2>
    <input type="text" id="producto" placeholder="Producto" />
    <input type="number" id="cantidad" placeholder="Cantidad" />
    <input type="number" id="precio" placeholder="Precio por unidad $" />
    <button onclick="agregarInventario()">Agregar</button>
    <button class="boton-secundario" onclick="borrarTodoInventario()">Borrar Todo</button>

    <table>
      <thead>
        <tr>
          <th>Producto</th>
          <th>Cantidad</th>
          <th>Precio ($)</th>
          <th>Total</th>
          <th>Acciones</th>
        </tr>
      </thead>
      <tbody id="tabla-inventario"></tbody>
    </table>
    <p class="resumen">Total invertido: $<span id="total-invertido">0.00</span></p>
  </section>

  <!-- Fiados -->
  <section>
    <h2>Fiados</h2>
    <input type="text" id="nombre" placeholder="Nombre" />
    <input type="number" id="deuda" placeholder="Deuda $" />
    <button onclick="agregarFiado()">Agregar</button>
    <button class="boton-secundario" onclick="borrarTodoFiados()">Borrar Todo</button>

    <table>
      <thead>
        <tr>
          <th>Nombre</th>
          <th>Deuda</th>
          <th>Acciones</th>
        </tr>
      </thead>
      <tbody id="tabla-fiados"></tbody>
    </table>
    <p class="resumen">Total deuda: $<span id="total-deuda">0.00</span></p>
  </section>

  <script>
    let inventario = JSON.parse(localStorage.getItem("inventario")) || [];
    let fiados = JSON.parse(localStorage.getItem("fiados")) || [];

    function guardarDatos() {
      localStorage.setItem("inventario", JSON.stringify(inventario));
      localStorage.setItem("fiados", JSON.stringify(fiados));
    }

    function agregarInventario() {
      const producto = document.getElementById("producto").value.trim();
      const cantidad = parseInt(document.getElementById("cantidad").value);
      const precio = parseFloat(document.getElementById("precio").value);

      if (!producto || isNaN(cantidad) || cantidad <= 0 || isNaN(precio) || precio <= 0) {
        alert("Por favor, ingresa todos los campos correctamente.");
        return;
      }

      inventario.push({ producto, cantidad, precio });
      guardarDatos();
      renderInventario();

      document.getElementById("producto").value = "";
      document.getElementById("cantidad").value = "";
      document.getElementById("precio").value = "";
    }

    function renderInventario() {
      const tabla = document.getElementById("tabla-inventario");
      tabla.innerHTML = "";
      let total = 0;

      inventario.forEach((item, index) => {
        const totalItem = item.cantidad * item.precio;
        total += totalItem;

        const fila = document.createElement("tr");
        fila.innerHTML = `
          <td contenteditable="true">${item.producto}</td>
          <td contenteditable="true">${item.cantidad}</td>
          <td contenteditable="true">${item.precio.toFixed(2)}</td>
          <td>$${totalItem.toFixed(2)}</td>
          <td>
            <button onclick="guardarEdicionInventario(${index}, this)">Guardar</button>
            <button class="boton-secundario" onclick="eliminarInventario(${index})">Eliminar</button>
          </td>
        `;
        tabla.appendChild(fila);
      });

      document.getElementById("total-invertido").textContent = total.toFixed(2);
    }

    function guardarEdicionInventario(index, btn) {
      const fila = btn.closest("tr");
      const celdas = fila.querySelectorAll("td");

      const producto = celdas[0].textContent.trim();
      const cantidad = parseInt(celdas[1].textContent.trim());
      const precio = parseFloat(celdas[2].textContent.trim());

      if (!producto || isNaN(cantidad) || isNaN(precio)) return;

      inventario[index] = { producto, cantidad, precio };
      guardarDatos();
      renderInventario();
    }

    function eliminarInventario(index) {
      if (confirm("¿Eliminar este producto?")) {
        inventario.splice(index, 1);
        guardarDatos();
        renderInventario();
      }
    }

    function borrarTodoInventario() {
      if (confirm("¿Seguro que deseas borrar todo el inventario?")) {
        inventario = [];
        guardarDatos();
        renderInventario();
      }
    }

    function agregarFiado() {
      const nombre = document.getElementById("nombre").value.trim();
      const deuda = parseFloat(document.getElementById("deuda").value);

      if (!nombre || isNaN(deuda) || deuda <= 0) {
        alert("Por favor, completa todos los campos correctamente.");
        return;
      }

      fiados.push({ nombre, deuda });
      guardarDatos();
      renderFiados();

      document.getElementById("nombre").value = "";
      document.getElementById("deuda").value = "";
    }

    function renderFiados() {
      const tabla = document.getElementById("tabla-fiados");
      tabla.innerHTML = "";
      let total = 0;

      fiados.forEach((item, index) => {
        total += item.deuda;

        const fila = document.createElement("tr");
        fila.innerHTML = `
          <td contenteditable="true">${item.nombre}</td>
          <td contenteditable="true">${item.deuda.toFixed(2)}</td>
          <td>
            <button onclick="guardarEdicionFiado(${index}, this)">Guardar</button>
            <button class="boton-secundario" onclick="eliminarFiado(${index})">Eliminar</button>
          </td>
        `;
        tabla.appendChild(fila);
      });

      document.getElementById("total-deuda").textContent = total.toFixed(2);
    }

    function guardarEdicionFiado(index, btn) {
      const fila = btn.closest("tr");
      const celdas = fila.querySelectorAll("td");

      const nombre = celdas[0].textContent.trim();
      const deuda = parseFloat(celdas[1].textContent.trim());

      if (!nombre || isNaN(deuda)) return;

      fiados[index] = { nombre, deuda };
      guardarDatos();
      renderFiados();
    }

    function eliminarFiado(index) {
      if (confirm("¿Eliminar esta persona?")) {
        fiados.splice(index, 1);
        guardarDatos();
        renderFiados();
      }
    }

    function borrarTodoFiados() {
      if (confirm("¿Seguro que deseas borrar todas las deudas?")) {
        fiados = [];
        guardarDatos();
        renderFiados();
      }
    }

    renderInventario();
    renderFiados();
  </script>
</body>

Desarrollado por Gustavo Enrique Vega Mercado
