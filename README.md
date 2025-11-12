<!DOCTYPE html>
<html lang="es">
<head>
    <meta charset="UTF-8">
    <title>Consulta de Notas</title>
    https://cdnjs.cloudflare.com/ajax/libs/PapaParse/5.3.2/papaparse.min.js</script>
    <style>
        body { font-family: Arial, sans-serif; margin: 20px; }
        input { margin: 5px; padding: 8px; width: 250px; }
        button { padding: 8px 12px; margin-top: 10px; }
        .resultado { margin-top: 20px; font-weight: bold; }
        table { border-collapse: collapse; margin-top: 10px; width: 100%; }
        th, td { border: 1px solid #ccc; padding: 8px; text-align: center; }
        th { background-color: #f2f2f2; }
    </style>
</head>
<body>
    <h2>Consulta de Notas</h2>
    <label>Número de Documento:</label><br>
    <input type="text" id="documento"><br>
    <label>Primer Apellido:</label><br>
    <input type="text" id="apellido"><br>
    <button onclick="consultarNota()">Consultar</button>

    <div class="resultado" id="resultado"></div>

    <script>
        let estudiantes = [];

        // ✅ Pega aquí el enlace público de tu Google Sheets en formato CSV
        const googleSheetCSV = "ENLACE_PUBLICO_A_TU_GOOGLE_SHEETS";

        // Cargar datos desde Google Sheets
        Papa.parse(googleSheetCSV, {
            download: true,
            header: true,
            complete: function(results) {
                estudiantes = results.data;
                console.log("Datos cargados:", estudiantes);
            }
        });

        function consultarNota() {
            const doc = document.getElementById('documento').value.trim();
            const ape = document.getElementById('apellido').value.trim().toLowerCase();
            const resultadoDiv = document.getElementById('resultado');

            const estudiante = estudiantes.find(e => e["Número de documento"] === doc && e["Apellidos"].toLowerCase().startsWith(ape));

            if (estudiante) {
                resultadoDiv.innerHTML = `
                    <p><strong>Estudiante:</strong> ${estudiante.Nombres} ${estudiante.Apellidos}</p>
                    <p><strong>Grado:</strong> ${estudiante.Grado} | <strong>Curso:</strong> ${estudiante.Curso}</p>
                    <table>
                        <tr><th>Periodo 1</th><th>Periodo 2</th><th>Periodo 3</th><th>Definitiva</th><th>Aprobó</th></tr>
                        <tr>
                            <td>${estudiante["Periodo 1"]}</td>
                            <td>${estudiante["Periodo 2"]}</td>
                            <td>${estudiante["Periodo 3"]}</td>
                            <td>${estudiante.Definitiva}</td>
                            <td>${estudiante["Aprobó (Sí/No)"]}</td>
                        </tr>
                    </table>
                `;
            } else {
                resultadoDiv.textContent = "No se encontró ningún registro con esos datos.";
            }
        }
    </script>
</body>
</html>
