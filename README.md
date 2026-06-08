Oscar - Exercícios MongoDB
Bem-vindo à base de dados do Oscar!

O Oscar é a premiação mais prestigiada do cinema mundial, realizada anualmente desde 1929 pela Academia de Artes e Ciências Cinematográficas. Nesta base de dados, você encontrará registros históricos de indicados e vencedores de diversas categorias ao longo de quase 100 anos de história do cinema.

Nestes exercícios, você vai explorar o banco de dados MongoDB do Oscar e responder perguntas que revelam insights fascinantes sobre a história do cinema, tendências de premiação, e momentos marcantes da indústria cinematográfica.

Dicas Gerais
Conversão de Dados: Use $toInt, $toBool para converter tipos
Arrays: Use $addToSet para valores únicos
Datas: Use $year para extrair o ano de campos de data
Performance: Para queries complexas, considere criar índices em campos frequentemente consultados
Boa prática: Sempre teste com .limit(5) antes de executar queries que retornam muitos resultados
Avaliação
Nível 1-4: Operações básicas e queries simples
Nível 5-8: Agregações e análise de dados
Nível 9-11: Queries complexas e pensamento analítico
Nível 12-14: Expertise avançada e pensamento estratégico
Objetivo de aprendizado: Ao completar todos os níveis, você será capaz de trabalhar com bases de dados históricas complexas, realizar análises estatísticas sofisticadas e extrair insights valiosos de grandes volumes de dados.

Nível 1: Primeiros Passos
Conhecendo a Base de Dados
1.1 Quantos registros existem na coleção de indicados ao Oscar?

Exemplo de resposta:

R: 1430 registros

db.oscar_indicados.countDocuments();
1.2 Quais são as diferentes categorias de premiação que existem no banco de dados? Liste todas as categorias únicas.

Exemplo de resposta:

R: 92 registros

db.oscar_indicados.distinct("categoria");
1.3 Qual foi o primeiro ano de cerimônia do Oscar registrado na base?

R: 1500

db.registros.find().skip(15).limit(1).sort({ano_cerimonia: -1})
1.4 Qual foi o último ano de cerimônia registrado na base?

1.5 Quantas cerimônias do Oscar estão registradas no total?

1.6 Atualize os registros da coleção com os dados do Oscar 2025 e 2026 (pesquise os vencedores e adicione-os).

Nível 2: Explorando Categorias
2.1 Quantas indicações existem para cada categoria? Agrupe por categoria e ordene da mais frequente para a menos frequente.

2.2 Qual categoria teve mais indicações ao longo da história do Oscar?

2.3 Qual categoria teve menos indicações ao longo da história?

2.4 A partir de que ano a categoria "ACTRESS" deixou de existir? (Dica: procure a última cerimônia com essa categoria)

2.5 Quais categorias existiam na primeira cerimônia (1928) e não existem mais hoje?

2.6 Liste todas as categorias que contêm a palavra "DIRECTING" no nome.

Nível 3: Atores e Atrizes Famosos
Natalie Portman
3.1 Quantas vezes Natalie Portman foi indicada ao Oscar?

3.2 Quantos Oscars Natalie Portman ganhou?

3.3 Em quais anos e por quais filmes Natalie Portman foi indicada?

3.4 Liste todas as indicações de Natalie Portman mostrando: ano, categoria, filme e se venceu.

Viola Davis
3.5 Quantas vezes Viola Davis foi indicada ao Oscar?

3.6 Quantos Oscars Viola Davis ganhou?

3.7 Por quais filmes Viola Davis foi indicada?

Amy Adams
3.8 Amy Adams já ganhou algum Oscar?

3.9 Quantas vezes Amy Adams foi indicada sem ganhar?

Denzel Washington
3.10 Denzel Washington já ganhou algum Oscar?

3.11 Quantas vezes Denzel Washington foi indicado ao Oscar?

3.12 Liste todos os Oscars que Denzel Washington ganhou (ano, categoria, filme).

Nível 4: Vencedores Históricos
4.1 Quem ganhou o primeiro Oscar para Melhor Atriz (ACTRESS)? Em que ano e por qual filme?

4.2 Quem ganhou o primeiro Oscar para Melhor Ator (ACTOR)? Em que ano e por qual filme?

4.3 Quantos vencedores existem ao todo na base de dados?

4.4 Liste todos os filmes que ganharam o Oscar de Melhor Filme (categoria "OUTSTANDING PICTURE" ou "BEST PICTURE").

4.5 Quantos filmes diferentes já ganharam o Oscar?

Nível 5: Análise de Indicações
5.1 Quais atores/atrizes foram indicados mais de uma vez? Liste o nome e o número de indicações.

5.2 Qual ator ou atriz tem o maior número de indicações na história do Oscar?

5.3 Quais atores foram indicados mais de 3 vezes, mas nunca ganharam?

5.4 Encontre todos os artistas que foram indicados em categorias diferentes (ex: ator e diretor).

5.5 Quantos indicados têm exatamente 1 indicação na história?

5.6 Qual o maior números de indicados em um único ano? Essa é uma pergunta franca.

Nível 6: Análise de Filmes
Toy Story
6.1 A série de filmes Toy Story ganhou Oscars em quais anos?

6.2 Quantas indicações a franquia Toy Story recebeu no total?

6.3 Em quais categorias os filmes Toy Story foram indicados?

Crash
6.4 Em qual edição do Oscar o filme "Crash" concorreu?

6.5 Quantas indicações o filme "Crash" recebeu?

6.6 "Crash" ganhou o Oscar de Melhor Filme?

Central do Brasil
6.7 O filme "Central do Brasil" aparece no banco de dados?

6.8 Se sim, quantas indicações "Central do Brasil" recebeu?
