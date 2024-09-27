<!DOCTYPE html>
<html lang="pt-BR">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Formulário Hospitalar</title>
    <link rel="stylesheet" href="formulario.css">

    body {
    font-family: Arial, sans-serif;
    background-color: #f4f4f4;
    padding: 20px;
}
form {
    max-width: 600px;
    margin: auto;
    background: #fff;
    padding: 20px;
    border-radius: 8px;
    box-shadow: 0 0 10px rgba(0, 0, 0, 0.1);
}
label {
    display: block;
    margin-bottom: 5px;
}
input, textarea, select {
    width: calc(100% - 30px);
    padding: 8px;
    margin-bottom: 15px;
    border: 1px solid #ccc;
    border-radius: 4px;
}
#cep-container {
    display: flex;
    align-items: center;
}
#cep-container input {
    flex: 1;
}
#cep-container button {
    background-color: #4CAF50;
    color: white;
    padding: 8px 15px;
    border: none;
    border-radius: 4px;
    cursor: pointer;
    margin-left: 10px;
}
#cep-container button:hover {
    background-color: #45a049;
}
button[type="submit"] {
    background-color: #4CAF50;
    color: white;
    padding: 10px 15px;
    border: none;
    border-radius: 4px;
    cursor: pointer;
}
button[type="submit"]:hover {
    background-color: #45a049;
}

    <script>
               const cep = document.getElementById('cep').value.replace(/\D/g, '');
            if (cep.length !== 8) {
                alert('CEP inválido!');
                return;
            }

            fetch("https: viacep.com.br/ws/${cep}/json/")
                .then(response => response.json())
                .then(data => {
                    if (data.erro) {
                        alert('CEP não encontrado!');
                        return;
                    }

                    document.getElementById('logradouro').value = data.logradouro || '';
                    document.getElementById('bairro').value = data.bairro || '';
                    document.getElementById('cidade').value = data.localidade || '';
                    document.getElementById('estado').value = data.uf || '';
                })
                .catch(error => {
                    alert('Erro ao buscar CEP!');
                    console.error('Erro:', error);
                });



    </script>
</head>
<body>
    <h2>Formulário Hospitalar</h2>
    <form action="/submit-form" method="POST">
        <fieldset>
            <legend>Informações do Paciente</legend>
            <label for="name">Nome Completo:</label>
            <input type="text" id="name" name="name" required>

            <label for="dob">Data de Nascimento:</label>
            <input type="date" id="dob" name="dob" required>

            <label for="gender">Gênero:</label>
            <select id="gender" name="gender" required>
                <option value="male">Masculino</option>
                <option value="female">Feminino</option>
                <option value="other">Outro</option>
            </select>

            <label for="phone">Telefone de Contato:</label>
            <input type="tel" id="phone" name="phone" required>

            <label for="email">Email:</label>
            <input type="email" id="email" name="email" required>
        </fieldset>

        <fieldset>
            <legend>Endereço</legend>
            <div id="cep-container">
                <input type="text" id="cep" name="cep" placeholder="CEP" required>
                <button type="button" onclick="buscaCep()">Atualizar Endereço</button>
            </div>

            <label for="logradouro">Logradouro:</label>
            <input type="text" id="logradouro" name="logradouro">

            <label for="bairro">Bairro:</label>
            <input type="text" id="bairro" name="bairro" required readonly>

            <label for="cidade">Cidade:</label>
            <input type="text" id="cidade" name="cidade" required readonly>

            <label for="estado">Estado:</label>
            <input type="text" id="estado" name="estado" required readonly>
        </fieldset>

        <fieldset>
            <legend>Histórico Médico</legend>
            <label for="medical-history">Histórico do paciente:</label>
            <textarea id="medical-history" name="medical_history" rows="4" required></textarea>

            <label for="allergies">Alergias:</label>
            <input type="text" id="allergies" name="allergies">

            <label for="current-medication">Medicação Atual:</label>
            <input type="text" id="current-medication" name="current_medication">
        </fieldset>

        <fieldset>
            <legend>Consentimento para Tratamento</legend>
            <label>
                <input type="checkbox" id="consent" name="consent" required>
                Concordo com os termos de tratamento e a coleta de dados para fins médicos.
            </label>
        </fieldset>

        <button type="submit">Enviar Formulário</button>
    </form>
</body>
</html>
