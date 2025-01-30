# **CAP Exercício 1: Definição do Modelo de Dados (`airline.cds` e `cep.cds`)**  

## **Objetivo**  
Neste exercício, você irá definir o **modelo de dados** do **Sistema de Gestão Aérea** utilizando **Core Data Services (CDS)**. O modelo será dividido em dois arquivos:  

- **`airline.cds`**: Contém as entidades relacionadas à gestão de companhias aéreas, aeronaves, aeroportos, voos, passageiros e reservas.  
- **`cep.cds`**: Contém a entidade `Cep`, que armazena dados de endereçamento para companhias e passageiros.  

Ao final desta etapa, o sistema terá um **modelo de dados estruturado**, permitindo a implementação dos serviços no próximo exercício.  

---

## **Passo 1: Criar os Arquivos de Modelo de Dados (`airline.cds` e `cep.cds`)**  

### **1. Criando o Arquivo `airline.cds`**  
1. No diretório do projeto, vá para a pasta `db` e crie o arquivo `airline.cds`:  

![image](https://github.com/user-attachments/assets/eab98d96-8c71-4869-940b-297d80b499ec)

2. Abra o arquivo `airline.cds` e adicione as entidades relacionadas ao sistema aéreo:

```cds
using {managed} from '@sap/cds/common';

namespace btpexp.airlines;

@assert.unique: {
    companhia_icao        : [icao],
    companhia_razao_social: [razao_social],
    companhia_cnpj        : [cnpj],
    companhia_telefone    : [telefone],
    companhia_email       : [email]
}
entity Companhia {
    key id_companhia          : UUID;
        icao                  : String(3)  @mandatory;
        razao_social          : String     @mandatory;
        iata                  : String(2);
        representante_legal   : String;
        pais_sede             : String;
        cnpj                  : String(14) @mandatory;
        endereco              : String;
        cidade                : String;
        uf                    : String(2);
        cep                   : String(8)  @mandatory;
        telefone              : String;
        email                 : String;
        decisao_operacional   : String;
        atividades_areas      : String;
        data_decisao_operacao : Date;
        validade_operacional  : Date;

        // Associação com a tabela de propriedade das aeronaves
        propriedade_aeronaves : Association to many PropriedadeAeronave
                                    on propriedade_aeronaves.id_companhia = id_companhia;
}

@assert.unique: {tipo_aeronave: [
    marca,
    ds_modelo,
    nr_serie
]}
entity Aeronave {
    key id_aeronave           : UUID;
        marca                 : String  @mandatory;
        ds_modelo             : String  @mandatory;
        nr_serie              : String  @mandatory;
        cd_categoria          : String;
        cd_tipo               : String;
        nm_fabricante         : String;
        cd_cls                : String;
        nr_pmd                : Decimal(10, 2);
        cd_tipo_icao          : String(4);
        nr_assentos_executivo : Integer default 0;
        nr_assentos_economico : Integer @mandatory;
        nr_assentos_max       : Integer @mandatory;
        nr_ano_fabricacao     : Integer;
        tp_motor              : String;
        qt_motor              : Integer;
        tp_pouso              : String;

        // Associação com a tabela de propriedade das aeronaves
        propriedade_aeronaves : Association to many PropriedadeAeronave
                                    on propriedade_aeronaves.id_aeronave = id_aeronave;
}

@assert.unique: {propriedade_aeronave_companhia: [
    id_companhia,
    id_aeronave
]}
entity PropriedadeAeronave {
    key id_propriedade_aeronave : UUID;
        id_companhia            : UUID @mandatory;
        id_aeronave             : UUID @mandatory;
        proprietario            : String;
        sg_uf                   : String(2) @mandatory;
        cpf_cnpj                : String(14) @mandatory;
        nm_operador             : String;
        nr_cert_matricula       : String;
        dt_validade_cva         : Date;
        dt_validade_ca          : Date;
        dt_canc                 : Date;
        cd_interdicao           : String;
        ds_gravame              : String;
        dt_matricula            : Date;

        // Associações com as entidades relacionadas
        companhia               : Association to Companhia
                                      on companhia.id_companhia = id_companhia @assert.target;
        aeronave                : Association to Aeronave
                                      on aeronave.id_aeronave = id_aeronave @assert.target;
}

@assert.unique: {aeroporto_icao: [icao]}
entity Aeroporto {
    key id_aeroporto     : UUID;
        icao             : String(4) @mandatory;
        nome             : String    @mandatory;
        cidade           : String    @mandatory;
        estado           : String;
        pais             : String;

        // Associação com conexões de origem (aeroporto origem)
        conexoes_origem  : Association to many Conexao
                               on conexoes_origem.id_aeroporto_origem = id_aeroporto;

        // Associação com conexões de destino (aeroporto destino)
        conexoes_destino : Association to many Conexao
                               on conexoes_destino.id_aeroporto_destino = id_aeroporto;
}

@assert.unique: {origem_destino: [
    id_aeroporto_origem,
    id_aeroporto_destino
]}
entity Conexao {
    key id_conexao           : UUID;
        id_aeroporto_origem  : UUID @mandatory;
        id_aeroporto_destino : UUID @mandatory;

        // Composição com Aeroporto de origem
        origem               : Composition of Aeroporto
                                   on origem.id_aeroporto = id_aeroporto_origem @assert.target;
        // Composição com Aeroporto de destino
        destino              : Composition of Aeroporto
                                   on destino.id_aeroporto = id_aeroporto_destino @assert.target;
}

@assert.unique: {
    nr_voo_companhia    : [
        id_companhia,
        id_conexao,
        nr_voo,
        partida_prevista,
        chegada_prevista
    ],
    aeronave_horario_voo: [
        id_aeronave,
        partida_prevista,
        chegada_prevista
    ]
}
entity HorarioVoo : managed {
    key id_horario_voo        : UUID;
        id_companhia          : UUID @mandatory;
        id_conexao            : UUID @mandatory;
        id_aeronave           : UUID @mandatory;
        nr_voo                : String  @mandatory;
        partida_prevista      : Time    @mandatory;
        chegada_prevista      : Time    @mandatory;
        data                  : Date    @mandatory;
        nr_assentos_executivo : Integer default 0;
        nr_assentos_economico : Integer @mandatory;
        nr_assentos_max       : Integer @mandatory;
        partida_real          : Time;
        chegada_real          : Time;
        situacao_voo          : String;
        situacao_partida      : String;
        situacao_chegada      : String;

        // Associação com a Companhia
        companhia             : Association to Companhia
                                    on companhia.id_companhia = id_companhia;
}

@assert.unique: {
    passageiro_cpf     : [cpf],
    passageiro_email   : [email],
    passageiro_telefone: [telefone]
}
entity Passageiro : managed {
    key id_passageiro   : UUID;
        cpf             : String(11) @mandatory;
        nome            : String;
        email           : String;
        telefone        : String;
        data_nascimento : Date @mandatory;
        endereco        : String;

        // Associação com as reservas de passagem
        reservas        : Association to many ReservaPassagem
                              on reservas.id_passageiro = id_passageiro;
}

entity ReservaPassagem : managed {
    key id_reserva     : UUID;
        id_horario_voo : UUID;
        id_passageiro  : UUID;
        assento        : String;
        classe         : String;
        status         : String;
        data_reserva   : Date;
        preco          : Decimal(10, 2);

        @odata.etag
        modifiedAt     : Timestamp;

        // Associação com Passageiro
        passageiro     : Association to Passageiro
                             on passageiro.id_passageiro = id_passageiro @assert.target;

        // Associação com Horário de Voo
        horario_voo    : Association to HorarioVoo
                             on horario_voo.id_horario_voo = id_horario_voo @assert.target;
}
```

---

### **2. Criando o Arquivo `cep.cds`**  
1. No diretório `db`, crie o arquivo `cep.cds`:  

![image](https://github.com/user-attachments/assets/3c16dc3b-0f71-4a2d-8b73-853e77d96374)
   
2. Abra o arquivo `cep.cds` e adicione a estrutura para armazenar dados de CEP:

```cds
namespace btpexp.esquema;

entity Cep {
  key CEP : String(9);
  logradouro : String;
  complemento : String;
  bairro : String;
  localidade : String;
  uf : String(2);
  estado : String;
  regiao : String;
  ibge : String;
  gia : String;
  ddd : String(3);
  siafi : String;
}
```

---

## **Passo 2: Validar os Modelos de Dados**
Agora, valide se os modelos foram criados corretamente executando o seguinte comando na raiz do projeto:

```sh
cds compile db
```

Se não houver erros, os modelos foram reconhecidos com sucesso.

---

## **Passo 3: Implantar os Modelos no Banco de Dados**
Para garantir que o banco de dados esteja sincronizado com os modelos definidos, execute os seguintes comandos:

```sh
npm install
```

Isso vai fazer instalar todas as **dependencias necessárias** para a aplicação funcionar corretamente.

```sh
cds deploy --to sqlite
```

Isso criará o banco de dados **SQLite** com as tabelas correspondentes às entidades definidas.

![image](https://github.com/user-attachments/assets/a7fbb6fb-15f7-45e6-90ad-a43527dd87bb)

---

## **Passo 4: Testar o Modelo**
1. Inicie o servidor CAP:  
   ```sh
   cds watch
   ```
2. No navegador, acesse `http://localhost:4004` e verifique se as entidades foram geradas corretamente.

![image](https://github.com/user-attachments/assets/74506a4d-5316-4741-9be7-f934addf3ab2)

---

## **Conclusão**
Agora que o **modelo de dados** foi definido e implantado, podemos prosseguir para o [**CAP Exercício 2**](https://github.com/ViniciusInfinitfy/btp-experience2025-AD267/tree/main/exercises/ex2), onde carregaremos **dados iniciais** no sistema utilizando arquivos `.csv`.  
