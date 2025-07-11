<?php
error_reporting(E_ALL);
ini_set('display_errors', 1);

$conn = pg_connect("host=localhost dbname=tiendadb user=tiendauser password=clave123");

if (!$conn) {
    echo "Error de conexión.";
    exit;
}

if ($_SERVER['REQUEST_METHOD'] == 'POST' && !empty($_POST['nombre']) && !empty($_POST['precio'])) {
    $nombre = $_POST['nombre'];
    $precio = $_POST['precio'];
    pg_query_params($conn, "INSERT INTO productos (nombre, precio) VALUES ($1, $2)", array($nombre, $precio));

header("Location: " . $_SERVER['PHP_SELF']);
    exit;
}

$result = pg_query($conn, "SELECT * FROM productos");
?>

<!DOCTYPE html>
<html>
<head>
    <title>Mi Tienda Sencilla</title>
</head>
<body>
    <h1>Lista de Productos</h1>
    <form method="POST">
        Nombre: <input type="text" name="nombre">
        Precio: <input type="text" name="precio">
        <input type="submit" value="Agregar">
    </form>
    <ul>
    <?php while ($row = pg_fetch_assoc($result)): ?>
        <li><?php echo htmlspecialchars($row['nombre']); ?> - $<?php echo htmlspecialchars($row['precio']); ?></li>
    <?php endwhile; ?>
    </ul>
</body>
</html>

