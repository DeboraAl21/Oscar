# Nível 1 – Primeiros Passos

## 1.1 Quantos registros existem na coleção de indicados ao Oscar?

R: 11.104 registros.

```javascript
db.oscar_indicados.countDocuments()
```

---

## 1.2 Quais são as diferentes categorias de premiação que existem no banco de dados?

R: 122 categorias diferentes.

```javascript
db.oscar_indicados.distinct("categoria")
```

Para obter a quantidade:

```javascript
db.oscar_indicados.distinct("categoria").length
```

---

## 1.3 Qual foi o primeiro ano de cerimônia do Oscar registrado na base?

R: 1928.

```javascript
db.oscar_indicados.aggregate([
{
  $group: {
    _id: null,
    primeiroAno: {
      $min: "$ano_cerimonia"
    }
  }
}
])
```

---

## 1.4 Qual foi o último ano de cerimônia registrado na base?

R: 2026.

```javascript
db.oscar_indicados.aggregate([
{
  $group: {
    _id: null,
    ultimoAno: {
      $max: "$ano_cerimonia"
    }
  }
}
])
```

---

## 1.5 Quantas cerimônias do Oscar estão registradas no total?

R: 98 cerimônias diferentes.

```javascript
db.oscar_indicados.distinct(
  "ano_cerimonia"
).length
```

Ou:

```javascript
db.oscar_indicados.aggregate([
{
  $group: {
    _id: "$ano_cerimonia"
  }
},
{
  $count: "totalCerimonias"
}
])
```

---

## 1.6 Atualize os registros da coleção com os dados do Oscar 2025 e 2026

Exemplo de inserção:

```javascript
db.oscar_indicados.insertMany([
{
  ano_cerimonia: 2025,
  categoria: "BEST PICTURE",
  nome_do_filme: "Anora",
  nome_do_indicado: "Anora",
  vencedor: true
},
{
  ano_cerimonia: 2026,
  categoria: "BEST PICTURE",
  nome_do_filme: "Anora",
  nome_do_indicado: "Anora",
  vencedor: true
}
])
```
# Nível 2 – Explorando Categorias

## 2.1 Quantas indicações existem para cada categoria? Agrupe por categoria e ordene da mais frequente para a menos frequente.

```javascript
db.oscar_indicados.aggregate([
{
  $group: {
    _id: "$categoria",
    totalIndicacoes: { $sum: 1 }
  }
},
{
  $sort: {
    totalIndicacoes: -1
  }
}
])
```

R: A consulta retorna todas as categorias ordenadas da mais frequente para a menos frequente.

---

## 2.2 Qual categoria teve mais indicações ao longo da história do Oscar?

R: DIRECTING, com 479 indicações.

```javascript
db.oscar_indicados.aggregate([
{
  $group: {
    _id: "$categoria",
    total: { $sum: 1 }
  }
},
{
  $sort: { total: -1 }
},
{
  $limit: 1
}
])
```

Resultado:

```json
{
  "_id": "DIRECTING",
  "total": 479
}
```

---

## 2.3 Qual categoria teve menos indicações ao longo da história?

R: SPECIAL ACHIEVEMENT AWARD (Sound Effects), com apenas 1 indicação.

```javascript
db.oscar_indicados.aggregate([
{
  $group: {
    _id: "$categoria",
    total: { $sum: 1 }
  }
},
{
  $sort: { total: 1 }
},
{
  $limit: 1
}
])
```

Resultado:

```json
{
  "_id": "SPECIAL ACHIEVEMENT AWARD (Sound Effects)",
  "total": 1
}
```

---

## 2.4 A partir de que ano a categoria "ACTRESS" deixou de existir?

R: A última ocorrência da categoria ACTRESS foi em 1976.

```javascript
db.oscar_indicados.find(
{
  categoria: "ACTRESS"
})
.sort({ ano_cerimonia: -1 })
.limit(1)
```

Ou:

```javascript
db.oscar_indicados.aggregate([
{
  $match: {
    categoria: "ACTRESS"
  }
},
{
  $group: {
    _id: null,
    ultimoAno: {
      $max: "$ano_cerimonia"
    }
  }
}
])
```

Resultado:

```json
{
  "ultimoAno": 1976
}
```

## 2.5 Quais categorias existiam na primeira cerimônia (1928) e não existem mais hoje?

R: Para descobrir corretamente, compare as categorias de 1928 com as categorias mais recentes da base.

```javascript
const categorias1928 = db.oscar_indicados.distinct(
  "categoria",
  { ano_cerimonia: 1928 }
);

const categoriasAtuais = db.oscar_indicados.distinct(
  "categoria",
  { ano_cerimonia: 2026 }
);

categorias1928.filter(
  cat => !categoriasAtuais.includes(cat)
);
```

Exemplos que costumam aparecer:

* ART DIRECTION
* ENGINEERING EFFECTS
* OUTSTANDING PICTURE
* WRITING

(Os nomes exatos dependem da versão da base.)

---


Nível 3 – Atores e Atrizes Famosos
3.5 Quantas vezes Viola Davis foi indicada ao Oscar?

R: 4 indicações.

db.oscar_indicados.countDocuments({
  nome_do_indicado: "Viola Davis"
})
3.6 Quantos Oscars Viola Davis ganhou?

R: 1 Oscar.

db.oscar_indicados.countDocuments({
  nome_do_indicado: "Viola Davis",
  vencedor: true
})
3.7 Por quais filmes Viola Davis foi indicada?

R:

Doubt
The Help
Fences
Ma Rainey's Black Bottom
db.oscar_indicados.find(
  { nome_do_indicado: "Viola Davis" },
  { nome_do_filme: 1, _id: 0 }
)
3.8 Amy Adams já ganhou algum Oscar?

R: Não.

db.oscar_indicados.countDocuments({
  nome_do_indicado: "Amy Adams",
  vencedor: true
})
3.9 Quantas vezes Amy Adams foi indicada sem ganhar?

R: 6 vezes.

db.oscar_indicados.countDocuments({
  nome_do_indicado: "Amy Adams",
  vencedor: false
})
3.10 Denzel Washington já ganhou algum Oscar?

R: Sim, 2 Oscars.

db.oscar_indicados.countDocuments({
  nome_do_indicado: "Denzel Washington",
  vencedor: true
})
3.11 Quantas vezes Denzel Washington foi indicado ao Oscar?

R: 9 indicações.

db.oscar_indicados.countDocuments({
  nome_do_indicado: "Denzel Washington"
})
3.12 Liste todos os Oscars que Denzel Washington ganhou

R:

Ano	Categoria	Filme
1990	ACTOR IN A SUPPORTING ROLE	Glory
2002	ACTOR IN A LEADING ROLE	Training Day
db.oscar_indicados.find(
{
  nome_do_indicado: "Denzel Washington",
  vencedor: true
},
{
  ano_cerimonia: 1,
  categoria: 1,
  nome_do_filme: 1,
  _id: 0
})

Nível 4 – Vencedores Históricos
4.1 Quem ganhou o primeiro Oscar para Melhor Atriz (ACTRESS)?

R:

Atriz: Janet Gaynor
Filme: 7th Heaven
Ano: 1928
db.oscar_indicados.find({
  categoria: "ACTRESS",
  vencedor: true
})
.sort({ ano_cerimonia: 1 })
.limit(1)
4.2 Quem ganhou o primeiro Oscar para Melhor Ator (ACTOR)?

R:

Ator: Emil Jannings
Filme: The Last Command
Ano: 1928
db.oscar_indicados.find({
  categoria: "ACTOR",
  vencedor: true
})
.sort({ ano_cerimonia: 1 })
.limit(1)
4.3 Quantos vencedores existem ao todo na base?

R: 2.507 vencedores.

db.oscar_indicados.countDocuments({
  vencedor: true
})
4.5 Quantos filmes diferentes já ganharam o Oscar?

R: 1.354 filmes diferentes.

db.oscar_indicados.distinct(
  "nome_do_filme",
  { vencedor: true }
).length



## 4.4 Liste todos os filmes que ganharam o Oscar de Melhor Filme (categoria "OUTSTANDING PICTURE" ou "BEST PICTURE")

```javascript
db.oscar_indicados.find(
{
  categoria: {
    $in: [
      "OUTSTANDING PICTURE",
      "BEST PICTURE"
    ]
  },
  vencedor: true
},
{
  ano_cerimonia: 1,
  nome_do_filme: 1,
  _id: 0
}
).sort({ ano_cerimonia: 1 })
```

Alguns exemplos encontrados:

| Ano  | Filme                                         |
| ---- | --------------------------------------------- |
| 1928 | Wings                                         |
| 1929 | The Broadway Melody                           |
| 1930 | All Quiet on the Western Front                |
| 1940 | Rebecca                                       |
| 1972 | The Godfather                                 |
| 1994 | Forrest Gump                                  |
| 1997 | Titanic                                       |
| 2003 | The Lord of the Rings: The Return of the King |
| 2019 | Parasite                                      |
| 2024 | Oppenheimer                                   |

---

## 5.1 Quais atores/atrizes foram indicados mais de uma vez?

```javascript
db.oscar_indicados.aggregate([
{
  $group: {
    _id: "$nome_do_indicado",
    indicacoes: { $sum: 1 }
  }
},
{
  $match: {
    indicacoes: { $gt: 1 }
  }
},
{
  $sort: {
    indicacoes: -1
  }
}
])
```

Exemplos:

| Nome              | Indicações |
| ----------------- | ---------- |
| Meryl Streep      | 21         |
| Katharine Hepburn | 12         |
| Jack Nicholson    | 12         |
| Bette Davis       | 11         |
| Denzel Washington | 9          |
| Glenn Close       | 8          |
| Amy Adams         | 6          |
| Natalie Portman   | 3          |

---

## 5.4 Encontre todos os artistas que foram indicados em categorias diferentes

Exemplo: alguém indicado como ator e também diretor.

```javascript
db.oscar_indicados.aggregate([
{
  $group: {
    _id: "$nome_do_indicado",
    categorias: {
      $addToSet: "$categoria"
    }
  }
},
{
  $project: {
    categorias: 1,
    quantidadeCategorias: {
      $size: "$categorias"
    }
  }
},
{
  $match: {
    quantidadeCategorias: { $gt: 1 }
  }
},
{
  $sort: {
    quantidadeCategorias: -1
  }
}
])
```

Exemplos famosos encontrados:

* Clint Eastwood

  * ACTOR
  * DIRECTING

* Kenneth Branagh

  * ACTOR
  * DIRECTING
  * WRITING

* Woody Allen

  * DIRECTING
  * WRITING

* George Clooney

  * ACTOR
  * ACTOR IN A SUPPORTING ROLE
  * DIRECTING

* Warren Beatty

  * ACTOR
  * DIRECTING
  * WRITING
5.5 Quantos indicados têm exatamente 1 indicação?

R: 5.731 indicados.

db.oscar_indicados.aggregate([
{
  $group: {
    _id: "$nome_do_indicado",
    total: { $sum: 1 }
  }
},
{
  $match: {
    total: 1
  }
},
{
  $count: "quantidade"
}
])
5.6 Qual o maior número de indicados em um único ano?

R: 1943, com 186 registros.

db.oscar_indicados.aggregate([
{
  $group: {
    _id: "$ano_cerimonia",
    total: { $sum: 1 }
  }
},
{
  $sort: { total: -1 }
},
{
  $limit: 1
}
])


Nível 6 – Análise de Filmes
6.1 A série Toy Story ganhou Oscars em quais anos?

R:

2011 (Toy Story 3)
2020 (Toy Story 4)
db.oscar_indicados.find({
  nome_do_filme: { $regex: "Toy Story" },
  vencedor: true
})
6.2 Quantas indicações a franquia Toy Story recebeu?

R: 11 indicações.

db.oscar_indicados.countDocuments({
  nome_do_filme: {
    $regex: "Toy Story"
  }
})
6.3 Em quais categorias Toy Story foi indicado?

R:

ANIMATED FEATURE FILM
BEST PICTURE
MUSIC (Original Musical or Comedy Score)
MUSIC (Original Song)
SOUND EDITING
WRITING (Adapted Screenplay)
WRITING (Screenplay Written Directly for the Screen)
db.oscar_indicados.distinct(
  "categoria",
  {
    nome_do_filme: {
      $regex: "Toy Story"
    }
  }
)
6.4 Em qual edição do Oscar o filme Crash concorreu?

R: Oscar de 2006.

db.oscar_indicados.find(
{
  nome_do_filme: "Crash"
},
{
  ano_cerimonia: 1,
  _id: 0
})
6.5 Quantas indicações o filme Crash recebeu?

R: 6 indicações.

db.oscar_indicados.countDocuments({
  nome_do_filme: "Crash"
})
6.6 Crash ganhou o Oscar de Melhor Filme?

R: Sim.

Categoria: BEST PICTURE.

db.oscar_indicados.find({
  nome_do_filme: "Crash",
  categoria: "BEST PICTURE",
  vencedor: true
})
6.7 O filme Central do Brasil aparece no banco?

R: Não.

6.8 Se sim, quantas indicações recebeu?

R: 0 registros encontrados.

db.oscar_indicados.find({
  nome_do_filme: "Central do Brasil"
})


