# Controle-de-Pacientes
Sistema de Controle de Pacientes desenvolvido em HTML, CSS e JavaScript.  O projeto permite:  Cadastro de pacientes Organização por convênios/bancos Cálculo automático de valores Soma total por convênio Exclusão de registros Salvamento automático no navegador usando LocalStorage
<!DOCTYPE html>
<html lang="pt-BR">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Controle de Pacientes</title>

<style>

body{
    font-family: Arial, sans-serif;
    background: #1e1e1e;
    color: white;
    padding: 30px;
}

h1{
    text-align: center;
}

.container{
    background: #2b2b2b;
    padding: 20px;
    border-radius: 15px;
    max-width: 1000px;
    margin: auto;
}

input, select{
    padding: 10px;
    margin: 5px;
    border: none;
    border-radius: 8px;
    width: 180px;
}

button{
    padding: 10px 20px;
    border: none;
    border-radius: 8px;
    background: #00b894;
    color: white;
    cursor: pointer;
    font-weight: bold;
}

button:hover{
    background: #00a383;
}

table{
    width: 100%;
    border-collapse: collapse;
    margin-top: 20px;
}

th, td{
    border: 1px solid #444;
    padding: 12px;
    text-align: center;
}

th{
    background: #00b894;
}

tr:nth-child(even){
    background: #333;
}

.deleteBtn{
    background: red;
}

.deleteBtn:hover{
    background: darkred;
}

</style>
</head>
<body>

<div class="container">

<h1>Controle de Pacientes</h1>

<select id="convenio">
<option value="">Selecione o convênio</option>
<option>Banco do Brasil</option>
<option>Itaú</option>
<option>Santander</option>
<option>Bradesco</option>
<option>Caixa Econômica</option>
<option>Nubank</option>
<option>Inter</option>
<option>PicPay</option>
<option>BTG Pactual</option>
<option>Safra</option>
<option>Sicoob</option>
<option>Sicredi</option>
<option>C6 Bank</option>
<option>Original</option>
<option>Mercado Pago</option>
<option>Next</option>
<option>Neon</option>
<option>PagBank</option>
<option>Stone</option>
<option>XP Investimentos</option>
</select>

<input type="text" id="paciente" placeholder="Nome do paciente">

<input type="number" id="valorPago" placeholder="Valor pago">

<input type="number" id="atendimentos" placeholder="Atendimentos no mês">

<button onclick="adicionarPaciente()">
Adicionar
</button>

<table>

<thead>
<tr>
<th>Convênio</th>
<th>Paciente</th>
<th>Valor Pago</th>
<th>Atendimentos</th>
<th>Valor Total</th>
<th>Ações</th>
</tr>
</thead>

<tbody id="tabelaPacientes">

</tbody>

</table>

</div>

<script>

let tabela = document.getElementById("tabelaPacientes");

carregarDados();

function adicionarPaciente(){

    let convenio = document.getElementById("convenio").value;
    let paciente = document.getElementById("paciente").value;
    let valorPago = parseFloat(document.getElementById("valorPago").value);
    let atendimentos = parseInt(document.getElementById("atendimentos").value);

    if(!convenio || !paciente || !valorPago || !atendimentos){
        alert("Preencha todos os campos");
        return;
    }

    let total = valorPago * atendimentos;

    let dados = JSON.parse(localStorage.getItem("pacientes")) || [];

    dados.push({
        convenio,
        paciente,
        valorPago,
        atendimentos,
        total
    });

    localStorage.setItem("pacientes", JSON.stringify(dados));

    mostrarTabela();

    document.getElementById("convenio").value = "";
    document.getElementById("paciente").value = "";
    document.getElementById("valorPago").value = "";
    document.getElementById("atendimentos").value = "";
}

function mostrarTabela(){

    tabela.innerHTML = "";

    let dados = JSON.parse(localStorage.getItem("pacientes")) || [];

    let conveniosAgrupados = {};

    dados.forEach((item, index) => {

        if(!conveniosAgrupados[item.convenio]){
            conveniosAgrupados[item.convenio] = [];
        }

        conveniosAgrupados[item.convenio].push({
            ...item,
            index
        });

    });

    let todosBancos = [
        "Banco do Brasil",
        "Itaú",
        "Santander",
        "Bradesco",
        "Caixa Econômica",
        "Nubank",
        "Inter",
        "PicPay",
        "BTG Pactual",
        "Safra",
        "Sicoob",
        "Sicredi",
        "C6 Bank",
        "Original",
        "Mercado Pago",
        "Next",
        "Neon",
        "PagBank",
        "Stone",
        "XP Investimentos"
    ];

    todosBancos.forEach(convenio => {

        let linhaTitulo = tabela.insertRow();

        linhaTitulo.innerHTML = `
            <td colspan="6"
            style="
            background:#00b894;
            font-size:22px;
            font-weight:bold;
            text-align:left;
            padding:15px;
            ">
                📁 ${convenio}
            </td>
        `;

        if(conveniosAgrupados[convenio]){

            conveniosAgrupados[convenio].forEach((item) => {

                let linha = tabela.insertRow();

                linha.innerHTML = `
                    <td>${item.convenio}</td>
                    <td>${item.paciente}</td>
                    <td>R$ ${item.valorPago.toFixed(2)}</td>
                    <td>${item.atendimentos}</td>
                    <td>R$ ${item.total.toFixed(2)}</td>
                    <td>
                        <button class="deleteBtn"
                        onclick="removerLinha(${item.index})">
                            Excluir
                        </button>
                    </td>
                `;
            });

        } else {

            let linhaVazia = tabela.insertRow();

            linhaVazia.innerHTML = `
                <td colspan="6"
                style="
                color:gray;
                font-style:italic;
                ">
                    Nenhum paciente cadastrado
                </td>
            `;
        }

    });

}

function removerLinha(index){

    let dados = JSON.parse(localStorage.getItem("pacientes")) || [];

    dados.splice(index, 1);

    localStorage.setItem("pacientes", JSON.stringify(dados));

    mostrarTabela();
}

function carregarDados(){
    mostrarTabela();
}

</script>

</body>
</html>
