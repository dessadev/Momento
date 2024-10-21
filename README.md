# Momento


1. Quantos funcionarios da empresa Momento trabalham no departamento de vendas?

R: 10 funcionários.
Q: 'db.funcionarios.countDocuments(
    {departamento: ObjectId("85992103f9b3e0b3b3c1fe71")
    })'

--------------------------------------------------------------

2. Inclua suas próprias informações no departamento de Tecnologia da empresa.

Q: db.funcionarios.insertOne({"_id": ObjectId(),
            "nome": "Andressa Prudente Morais",
            "telefone": "988.150.1160",
            "email": "andressa.p@momento.org",
            "dataAdmissao": "2024-09-24",
            "cargo": "Desenvolvedor Full Stack",
            "salario": 12000,
            "departamento": ObjectId("85992103f9b3e0b3b3c1fe74"), })

--------------------------------------------------------------

3. Agora diga, quantos funcionários temos ao total na empresa?

R: 24 funcionários.
Q: db.funcionarios.countDocuments();

--------------------------------------------------------------

4. E quanto ao Departamento de Tecnologia?

R: 06 funcionários.
Q: db.funcionarios.countDocuments(
    {departamento: ObjectId("85992103f9b3e0b3b3c1fe74")
    })

--------------------------------------------------------------

5. Qual a média salarial do departamento de tecnologia?

R: A média é de R$ 5.800
Q: db.funcionarios.aggregate([
  {
    $match: {
      departamento: ObjectId("85992103f9b3e0b3b3c1fe74")
    }
  }, 
  {
  $group: {
    _id: null,
    salario: { $sum: "$salario" },
    totalfuncionarios: { $sum: 1 }
  }
},
{
  $project: {
    _id: 0,
    mediaSalarios: { $divide: ["$salario", "$totalfuncionarios"] }
  }
}
])

--------------------------------------------------------------

6. Quanto o departamento de Vendas gasta em salários?

R: R$ 95.100
Q: db.funcionarios.aggregate([
    {
      $match: {
        departamento: ObjectId("85992103f9b3e0b3b3c1fe71")
      }
    },
    {
      $group: {
        _id: null,
        salario: { $sum: "$salario" },
    
      }
    }
  ])

--------------------------------------------------------------

7. Um novo departamento foi criado. O departamento de Inovações. Ele será locado no Brasil. 
Por favor, adicione-o no banco de dados da empresa colocando quaisquer informações que você 
achar relevantes.
 
Q: db.departamentos.insertOne(
    {
        "_id": ObjectId(),
        "nome": "Inovacoes",
        "escritorio": ObjectId()
    })

    db.escritorios.insertOne(
    { 
        "_id": ObjectId("66f2d816d0ff1d4c041b6a82"),
        "nome": "Croft Offices",
        "endereco": "Rua dos Exploradores, 123, São Paulo, SP Brasil - 08454 - 020",
        "telefone": "+55 (11) 98765-4321",
        "pais": "BR",
        "suprimentos": [
            {
                "produto": "Cafeteiras",
                "quantidade": 7,
                "precoUnitario": 283.75
            },
            {
                "produto": "Kit de Papel A4",
                "quantidade": 50,
                "precoUnitario": 752.85
            },  
            {
                "produto": "Planners",
                "quantidade": 20,
                "precoUnitario": 500
            },
            {
                "produto": "Post-its",
                "quantidade": 50,
                "precoUnitario": 1000  
            },
            {
                "produto": "Computadores",
                "quantidade": 10,
                "precoUnitario": 5000
            } ]
    } )

--------------------------------------------------------------

8. O departamento de Inovações está sem funcionários. Inclua alguns colegas de turma nesse departamento.

Q: db.funcionarios.insertMany([
  {
   "_id": ObjectId(),
   "nome": "Murilo Silva",
   "telefone": "965.450.4190",
   "email": "murilo.s@momento.org",
   "dataAdmissao": "2024-09-24",
   "cargo": "Desenvolvedor Full Stack",
   "salario": 12000,
   "departamento": ObjectId("66f2d816d0ff1d4c041b6a81")
  },{
   "_id": ObjectId(),
   "nome": "Matheus Silva",
   "telefone": "465.498.2230",
   "email": "matheus.sil@momento.org",
   "dataAdmissao": "2024-09-24",
   "cargo": "Desenvolvedor Full Stack",
   "salario": 12000,
   "departamento": ObjectId("66f2d816d0ff1d4c041b6a81")
  },{
   "_id": ObjectId(),
   "nome": "Anna Cristina",
   "telefone": "931.914.9041",
   "email": "anna.tina@momento.org",
   "dataAdmissao": "2024-09-24",
   "cargo": "Desenvolvedor Full Stack",
   "salario": 12000,
   "departamento": ObjectId("66f2d816d0ff1d4c041b6a81")
  }
])

--------------------------------------------------------------

9. Quantos funcionarios a empresa Momento tem agora?

R: 27 funcionários.
Q: db.funcionarios.countDocuments();

--------------------------------------------------------------

10. Quantos funcionários da empresa Momento possuem conjuges?

R: 7 funcionários.
Q: db.funcionarios.aggregate([
  {
      $match: {
          "dependentes.conjuge": { $exists: true }
      }
  },
  {
      $count: "conjuges"
  }
]);

--------------------------------------------------------------

11. Qual a média salarial dos funcionários da empresa Momento, excluindo-se o CEO?

R: R$ 10.103
Q: db.funcionarios.aggregate([
  {
      $match: {
          cargo: { $not: /ceo/i }
      }
  },
  {
      $group: {
          _id: null,
          salario: { $sum: "$salario" },
          totalfuncionarios: { $sum: 1 }
        }
      },
      {
        $project: {
          _id: 0,
          mediaSalarios: { $divide: ["$salario", "$totalfuncionarios"] }
        }
      }
    ])

--------------------------------------------------------------

12. Qual a média salarial do departamento de tecnologia?

R: R$ 5.800
Q: db.funcionarios.aggregate([
  {
    $match: {
      departamento: ObjectId("85992103f9b3e0b3b3c1fe74")
    }
  }, 
  {
  $group: {
    _id: null,
    salario: { $sum: "$salario" },
    totalfuncionarios: { $sum: 1 }
  }
},
{
  $project: {
    _id: 0,
    mediaSalarios: { $divide: ["$salario", "$totalfuncionarios"] }
  }
}
])

--------------------------------------------------------------

13. Qual o departamento com a maior média salarial?

R: Executivo.
Q: db.funcionarios.aggregate([
  { $group:{ _id: "$departamento", totalsalario:{$sum:"$salario"}, 
  totalfuncionarios:{$sum:1} }}, 
  { $project:{ departamento: "$_id", _id: "$departamento", mediasalarial:{$divide:["$totalsalario","$totalfuncionarios"]} } },
   { $sort:{"mediasalarial":-1}},{$limit:1}
  
  ])

--------------------------------------------------------------

14. Qual o departamento com o menor número de funcionários?

R: CEO.
Q: db.funcionarios.aggregate([
  {
    $group: {
      _id: "$departamento",
      somafun: {$sum: 1}
    }
  },
  {
    $sort: {"somafun": 1}
  },
  {
    $limit: 1
  }
])

--------------------------------------------------------------

15. Pensando na relação quantidade e valor unitario, qual o produto mais valioso da empresa?
R: Sabre de Luz.
Q: db.vendas.aggregate([
  {
    $sort: {quantidade: -1}
  },
  {
    $sort: {precoUnitario: -1}
  },
  {
    $limit: 1
  }
])

---------------------------------------------------------------

16. Qual o produto mais vendido da empresa?
R: Uniforme de Moléculas Instáveis. 
Q: db.vendas.aggregate([
  {
    $sort: {quantidade: -1}
  },
  {
    $limit: 1
  }
])

---------------------------------------------------------------

17. Qual o produto menos vendido da empresa?
R: Uniforme do Superman. 
Q: db.vendas.aggregate([
  {
    $sort: {quantidade: 1}
  },
  {
    $limit: 1
  }
])

---------------------------------------------------------------
