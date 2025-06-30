<!DOCTYPE html>

<html lang="mg">

<head>

  <meta charset="UTF-8">

  <title>Fandohavana Adidy</title>

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



    #searchInput, #filterEtat {

      width: 100%;

      padding: 8px;

      margin: 10px 0;

      border-radius: 5px;

      border: 1px solid #ccc;

    }



    .delete-btn {

      color: red;

      cursor: pointer;

      font-weight: bold;

    }



    .total {

      text-align: right;

      margin-top: 15px;

      font-weight: bold;

    }



    .show-password {

      cursor: pointer;

      position: absolute;

      right: 10px;

      top: 10px;

      color: #666;

    }

  </style>

</head>

<body>



  <h2>Fandohavana Adidy</h2>



  <form id="adidyForm">

    <label for="taranaka">Taranaka:</label>

    <input type="text" id="taranaka" name="taranaka" required>



    <label for="anarana">Anarana sy Fanampiny:</label>

    <input type="text" id="anarana" name="anarana" required>



    <label for="taona">Taona:</label>

    <input type="number" id="taona" name="taona" min="1" required>



    <label for="tel">TÃ©lÃ©phone (03XXXXXXXX):</label>

    <input type="tel" id="tel" name="tel">



    <label for="daty">Daty fandoavana:</label>

    <input type="date" id="daty" name="daty" required>



    <label for="vola">Vola (MGA):</label>

    <input type="number" id="vola" name="vola" min="0" required>



    <label for="etat">Etat:</label>

    <select id="etat" name="etat" required>

      <option value="">--- Misafidiana ---</option>

      <option value="Paye">Paye</option>

      <option value="Non PayÃ©">Non PayÃ©</option>

    </select>



    <label for="password">Mot de Passe (Alefa):</label>

    <div style="position: relative;">

      <input type="password" id="password" name="password" required>

      <span id="togglePassword" class="show-password">&#128065;</span>

    </div>



    <button type="submit">Alefa</button>

  </form>



  <h3>Fikarohana:</h3>

  <input type="text" id="searchInput" placeholder="Tadiavo eto...">

  

  <label for="filterEtat">Filtre Etat:</label>

  <select id="filterEtat">

    <option value="">â€” Etat rehetra â€”</option>

    <option value="Paye">Paye</option>

    <option value="Non PayÃ©">Non PayÃ©</option>

  </select>



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

        <th>SupprimÃ©</th>

      </tr>

    </thead>

    <tbody></tbody>

  </table>



  <div class="total" id="totalVola"></div>



  <script>

    const form = document.getElementById('adidyForm');

    const telInput = document.getElementById('tel');

    const tableBody = document.querySelector('#tableData tbody');

    const searchInput = document.getElementById('searchInput');

    const filterEtat = document.getElementById('filterEtat');

    const totalVola = document.getElementById('totalVola');

    const togglePassword = document.getElementById('togglePassword');

    const passwordField = document.getElementById('password');



    togglePassword.addEventListener('click', function() {

      const type = passwordField.type === 'password' ? 'text' : 'password';

      passwordField.type = type;

      togglePassword.textContent = type === 'password' ? 'ðŸ‘ï¸' : 'ðŸ™ˆ';

    });



    function loadData() {

      const storedData = JSON.parse(localStorage.getItem('fandoavanaData')) || [];

      const searchValue = searchInput.value.toLowerCase();

      const etatFilter = filterEtat.value;



      tableBody.innerHTML = "";

      let total = 0;



      storedData

        .filter(data => 

          (data.anarana.toLowerCase().includes(searchValue) || data.taranaka.toLowerCase().includes(searchValue)) &&

          (etatFilter === "" || data.etat === etatFilter)

        )

        .forEach((data, index) => {

          const row = document.createElement('tr');

          row.innerHTML = `

            <td>${data.taranaka}</td>

            <td>${data.anarana}</td>

            <td>${data.taona}</td>

            <td>${data.tel || '-'}</td>

            <td>${data.daty}</td>

            <td>${data.vola}</td>

            <td>${data.etat}</td>

            <td><span class="delete-btn" onclick="deleteEntry(${index})">X</span></td>

          `;

          total += parseInt(data.vola);

          tableBody.appendChild(row);

        });



      totalVola.textContent = `Total Vola: ${total.toLocaleString()} MGA`;

    }



    searchInput.addEventListener('input', loadData);

    filterEtat.addEventListener('change', loadData);



    function deleteEntry(index) {

      const pass = prompt("Ampidiro ny mot de passe hanaovana suppression:");

      if (pass !== "rafaranetsa") {

        alert("Diso ny mot de passe!");

        return;

      }



      let data = JSON.parse(localStorage.getItem('fandoavanaData')) || [];

      data.splice(index, 1);

      localStorage.setItem('fandoavanaData', JSON.stringify(data));

      loadData();

    }



    form.addEventListener('submit', function(event) {

      event.preventDefault();



      const pass = passwordField.value;

      if (pass !== "RKTPSTR") {

        alert("Diso ny mot de passe!");

        return;

      }



      const formData = new FormData(form);

      const data = {};

      formData.forEach((value, key) => data[key] = value);



      if (data.tel && !/^03\d{8}$/.test(data.tel)) {

        alert("Tokony hanomboka amin'ny '03' ary 10 isa ny laharana.");

        return;

      }



      let fandoavanaData = JSON.parse(localStorage.getItem('fandoavanaData')) || [];

      fandoavanaData.push(data);

      localStorage.setItem('fandoavanaData', JSON.stringify(fandoavanaData));



      alert("Vita soa aman-tsara!");

      form.reset();

      loadData();

    });



    loadData();

  </script>



</body>

</html>
