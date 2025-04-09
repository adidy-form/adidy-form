<!DOCTYPE html>
<html lang="mg">
<head>
  <meta charset="UTF-8">
  <title>Gandohavana Adidy</title>
  <style>
    body {
      font-family: Arial, sans-serif;
      background: #f2f2f2;
      padding: 20px;
    }

    h2 {
      text-align: center;
      color: #333;
    }

    form {
      max-width: 500px;
      margin: auto;
      background: #fff;
      padding: 20px;
      border-radius: 10px;
      box-shadow: 0 0 10px rgba(0,0,0,0.1);
    }

    label {
      display: block;
      margin-top: 10px;
      font-weight: bold;
    }

    input, select {
      width: 100%;
      padding: 8px;
      margin-top: 5px;
      border-radius: 5px;
      border: 1px solid #ccc;
    }

    button {
      margin-top: 15px;
      background-color: #4CAF50;
      color: white;
      border: none;
      padding: 10px;
      width: 100%;
      border-radius: 5px;
      cursor: pointer;
      font-size: 16px;
    }

    button:hover {
      background-color: #45a049;
    }

    .error {
      color: red;
      font-size: 14px;
    }

    table {
      margin-top: 30px;
      width: 100%;
      border-collapse: collapse;
    }

    th, td {
      border: 1px solid #ccc;
      padding: 8px;
      text-align: left;
    }

    th {
      background-color: #f4f4f4;
    }

    #searchInput {
      width: 100%;
      padding: 8px;
      margin: 20px 0;
      border-radius: 5px;
      border: 1px solid #ccc;
    }
  </style>
</head>
<body>

  <h2>Gandohavana Adidy</h2>

  <form id="adidyForm">
    <label for="taranaka">Taranaka:</label>
    <input type="text" id="taranaka" name="taranaka" required>

    <label for="anarana">Anarana sy Fanampiny:</label>
    <input type="text" id="anarana" name="anarana" required>

    <label for="taona">Taona:</label>
    <input type="number" id="taona" name="taona" min="1" required>

    <label for="tel">Téléphone (03XXXXXXXX):</label>
    <input type="tel" id="tel" name="tel" required>
    <span id="telError" class="error"></span>

    <label for="daty">Daty fandoavana:</label>
    <input type="date" id="daty" name="daty" required>

    <label for="vola">Vola (MGA):</label>
    <input type="number" id="vola" name="vola" min="0" required>

    <label for="etat">Etat:</label>
    <select id="etat" name="etat" required>
      <option value="">--- Misafidiana ---</option>
      <option value="Paye">Paye</option>
      <option value="Non Payé">Non Payé</option>
    </select>

    <button type="submit">Alefa</button>
  </form>

  <h3>Fikarohana Anarana na Taranaka:</h3>
  <input type="text" id="searchInput" placeholder="Tadiavo eto...">

  <table id="tableData">
    <thead>
      <tr>
        <th>Taranaka</th>
        <th>Anarana</th>
        <th>Taona</th>
        <th>Tel</th>
        <th>Daty</th>
        <th>Vola</th>
        <th>Etat</th>
      </tr>
    </thead>
    <tbody></tbody>
  </table>

  <script>
    const form = document.getElementById('adidyForm');
    const telInput = document.getElementById('tel');
    const telError = document.getElementById('telError');
    const tableBody = document.querySelector('#tableData tbody');
    const searchInput = document.getElementById('searchInput');

    // Maka sy manivana angona amin’ny fikarohana
    function loadData() {
      const storedData = JSON.parse(localStorage.getItem('fandoavanaData')) || [];
      const searchValue = searchInput.value.toLowerCase();

      tableBody.innerHTML = "";

      storedData
        .filter(data => 
          data.anarana.toLowerCase().includes(searchValue) || 
          data.taranaka.toLowerCase().includes(searchValue)
        )
        .forEach(data => {
          const row = document.createElement('tr');
          row.innerHTML = `
            <td>${data.taranaka}</td>
            <td>${data.anarana}</td>
            <td>${data.taona}</td>
            <td>${data.tel}</td>
            <td>${data.daty}</td>
            <td>${data.vola}</td>
            <td>${data.etat}</td>
          `;
          tableBody.appendChild(row);
        });
    }

    // Alefa rehefa mitady
    searchInput.addEventListener('input', loadData);

    form.addEventListener('submit', function(event) {
      event.preventDefault();
      
      const tel = telInput.value;

      if (!/^03\d{8}$/.test(tel)) {
        telError.textContent = "Tokony hanomboka amin'ny '03' ary 10 isa ny laharana.";
        return;
      } else {
        telError.textContent = "";
      }

      const formData = new FormData(form);
      const data = {};
      formData.forEach((value, key) => data[key] = value);

      // Mitahiry ao anaty localStorage
      let fandoavanaData = JSON.parse(localStorage.getItem('fandoavanaData')) || [];
      fandoavanaData.push(data);
      localStorage.setItem('fandoavanaData', JSON.stringify(fandoavanaData));

      alert("Vita soa aman-tsara!");
      form.reset();
      loadData();
    });

    // Alaina avy hatrany ny angona amin'ny fanombohana
    loadData();
  </script>

</body>
</html>
