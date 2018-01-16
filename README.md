
# nossascidades
Nossas Cidades é um estudo realizado com a liguagem de programação Python e SQLite para verificar os recursos que os municípios brasileiros deixaram de investir nos últimos 20 anos. 
Neste estudo, você vai saber:
- Quanto dinheiro os municípios brasileiros deixaram de investir nos últimos 20 anos;
- Quais municípios mais desperdiçaram as oportunidades;
- Qual é o município brasileiro campeão, que investiu cada centavo que recebeu do Governo Federal;
- Recursos enviados para o exterior superaram valores de muitas cidades brasileiras.

Informação importante: como o arquivo base de estudo convenios.txt tem quase 200 mb não foi possível fazer o upload no Github, mas ele pode ser aberto por este link https://drive.google.com/file/d/1463dQnvNIaG-4vkI0NePRUHs28YqJKMc/view?usp=sharing

A explicação segue abaixo, mas para ver o arquivo completo com imagens e informações mais detalhadas acesse esse link:
https://drive.google.com/file/d/1jrmTBUfWlKk-a_hJcu9nTlh7tWFdO5A5/view?usp=sharing

Introdução

No Brasil existem muitos canais de desperdício do dinheiro público que são veiculados na imprensa rotineiramente, entre eles, o mais notável, o da corrupção. Enquanto a operação Lava-Jato e outras ações do Ministério Público denunciam crimes que envolvem bilhões de reais, o projeto Serenata de Amor alerta a sociedade sobre pequenos gastos ilegais do legislativo brasileiro que somados ao longo do tempo revelam um grande prejuízo. Neste trabalho será abordada uma outra forma de desperdício, a dos convênios realizados entre o Governo Federal e as prefeituras do Brasil e também com outros países. O objetivo é mostrar que em muitos casos o município tem uma demanda importante, o Governo Federal disponibiliza o dinheiro, mas por algum motivo, sejam questões políticas, falta de gestão, falta de qualificação de funcionários ou desinteresse, esse dinheiro, que já estava garantido ao município, não chega ao destino.

O que são os convênios?

O município que vai receber uma emenda parlamentar ou recurso de um programa federal tem a obrigatoriedade de celebrar um convênio com o governo. Este convênio prevê o valor do investimento, contrapartidas, prazos e obrigações que devem ser cumpridas pelo município para aplicar o recurso. Por meio do Portal da Transparência(http://www.portaldatransparencia.gov.br/convenios/) é possível ter acesso a todos os convênios firmados com o Governo Federal desde 1996. Para fazer a pesquisa é possível utilizar filtros, como estado, município, órgão que está liberando o recurso, etc. Mas fazer esta pesquisa navegando no próprio site pode ser um processo muito trabalhoso, já que estão cadastrados mais de 480 mil convênios. Portanto, para tornar esse estudo mais ágil foi baixado o arquivo em csv disponível no site http://www.portaldatransparencia.gov.br/convenios/ConveniosListaGeral.asp (é preciso rolar a tela para baixo até o final) e todos os dados foram verificados em linguagem de programação Python.

As informações	

Utilizando alguns programas simples criados em Python foi possível chegar a algumas informações muito interessantes, como:
- Quanto dinheiro os municípios brasileiros deixaram de investir nos últimos 20 anos;
- Quais municípios mais desperdiçaram as oportunidades;
- Qual é o município brasileiro campeão, que investiu cada centavo que recebeu do Governo Federal;
- Recursos enviados para o exterior superaram valores de muitas cidades brasileiras.
Quer descobrir? Então vamos prosseguir!

Informações do arquivo

Cada linha do csv baixado do Portal da Transparência gerou um dicionário. Confira este exemplo de um recurso destinado ao município de Lauro Muller, em Santa Catarina:

{'': '0', 'Valor Contrapartida': '55000.00', 'Nome Municipio': 'LAURO MULLER', 'Numero Original': '06159/2009', 'Valor Ultima Liberacao\t\t\t\t\t\t\t\t\t\t\t\t\t\t\t\t\t': '195000.00\t\t\t\t\t\t\t\t\t\t\t\t\t\t\t\t\t', 'Valor Convenio': '195000.00', 'Nome Convenente': 'MUNICIPIO DE LAURO MULLER', 'Tipo Ente Convenente': "'M' (municipal)", 'Nome Concedente': 'CAIXA ECONOMICA FEDERAL/MA', 'Data Publicacao': '2010-01-20', 'Situacao Convenio': 'Prestacao de Contas Aprovada', 'Data Ultima Liberacao': '2010-10-20', 'Codigo Concedente': '135098.0', 'Valor Liberado': '195000.00', 'Nome Orgao Superior': 'MINIST. DA AGRICUL..PECUARIA E ABASTECIMENTO', 'Codigo Convenente': '82558909000124', 'Data Inicio Vigencia': '2009-12-31', 'Numero Convenio': '900154', 'UF': 'SANTA CATARINA', 'Data Fim Vigencia': '2010-12-31', 'Objeto Convenio': 'AQUISICAO DE 01 (UM) CAMINHAO TRUQUE NOVO E 01 (UM) TANQUE DE CHAPA METALICA PARA LIMPEZA POR SUCCAO.', 'Codigo Orgao Superior': '22000', 'Codigo SIAFI Municipio': '8189'}

O primeiro valor do dicionário é zero porque esta linha é a primeira do arquivo e o Python ordenou automaticamente. Os dados mais importantes deste dicionário para a realização deste estudo foram: Valor Convenio, Situacao Convenio, Valor Liberado e Codigo SIAFI Municipio. O Valor Convenio é o recurso que o Governo Federal se comprometeu a liberar. O Valor Liberado é o que efetivamente foi transferido ao município. A chave Situacao Convenio, que será detalhada mais adiante, mostra como está a tramitação do convênio junto ao governo e, finalmente, o Codigo SIAFI Municipio. Essa é a identificação do município no Tesouro Nacional, fundamental para a extração dos dados, já que municípios de estados diferentes podem ter o mesmo nome e utilizar esse código evita erros.

Situação do Convênio

A situação do convênio foi a chave que permitiu detectar os recursos que foram aplicados, os que estão tramitando adequadamente e os que apresentam problemas. Eles seguem as seguintes classificações. As explicações sobre essas classificações estão disponíveis no site da Transparência http://www.portaltransparencia.gov.br/faleconosco/perguntas-tema-convenios.asp.

['Prestacao de Contas Aprovada', 'Anulado', 'Em Execucao', 'Prestacao de Contas Aprovada com Ressalvas', 'Aguardando Prestacao de Contas', 'Prestacao de Contas em Complementacao', 'Prestacao de Contas enviada para Analise', 'Inadimplente', 'Concluido', 'Assinado', 'Prestacao de Contas Iniciada por Antecipacao', 'Prestacao de Contas em Analise', 'Prestacao de Contas Rejeitada', 'Cancelado', 'Adimplente', 'Excluido', 'Inadimplencia Suspensa', 'Rescindido', 'Baixado', 'Arquivado']

Como já foi citado anteriormente, as prefeituras devem cumprir diversas obrigações, antes da liberação do recurso e durante a execução. Existem as etapas preliminares de apresentação de documentos e do projeto que será realizado. Tudo será conferido pelo órgão que faz a liberação do valor, na maioria dos casos a Caixa Econômica Federal. Em caso de problemas, correções são solicitadas. Já na etapa de execução do projeto a prefeitura é obrigada a enviar prestações de contas regularmente
Com base nestas informações já é possível compreender que se o município tem a situação do convênio classificada como adimplente, significa que toda a documentação e informações estão sendo enviadas dentro dos prazos. Já as prestações de contas têm diversas definições. Mesmo em casos com problemas na prestação de contas é possível corrigir e normalizar a situação, mas podem existir situações em que o município não fornece as informações necessárias e pode, mesmo com a obra concluída, ser penalizado a devolver o dinheiro ao governo. As situações dos convênios ainda serão abordadas neste estudo mais adiante.

Explorando os dados

Antes de iniciar a extração dos dados, o arquivo que serviria de base precisava ser preparado. Na chave do dicionário “Data Ultima Liberacao”, os municípios que não tiveram o convênio concluído têm esse campo em branco. Esse campo null estava impedindo o funcionamento do programa. Por isso, foi feita a troca do null pelo NaN utilizando Pandas (pandas_null.py). Utilizei outros arquivos provisórios até chegar ao convênios.txt, que é o repositório de toda esta pesquisa. 

Para conferir se os programas desenvolvidos em Python estavam funcionando adequadamente e nos fornecendo informações confiáveis, foi feita uma comparação entre os valores gerados em csv para cada UF e os disponíveis no site. Para facilitar a visualização dos dados, os valores foram convertidos para inteiros. Isso explica a pequena diferença entre os valores, que para o objetivo deste estudo não é significante.

Nos arquivos liberado_por_uf_1996_2017, em Python e csv, os valores retornados bateram com os apresentados no arquivo tela_09_01_2018.jpeg. Este print da tela é necessário porque os dados são atualizados semanalmente. A conferência destes dados deu confiança para seguir adiante. 

Valores totais conveniados, liberados e perdas

Os arquivos total_liberado e total_conveniado (csv e Python), mostram os valores totais em convênios celebrados desde 1996 e quanto foi liberado efetivamente. 

Note que exceto nas situações “Anulado” e “Assinado” em que os valores liberados são iguais a zero(no caso de “Assinado” o convênio é muito recente, por isso também ainda não houve liberação), há recursos liberados em outras classificações que apresentam problemas, como é o caso de “arquivado”, “excluído”, “baixado”, “cancelado” e “rescindido”. Há casos em que um convênio “rescindido”, por exemplo, têm valor liberado zero e outros em que o sistema informa pagamento integral do recurso. Pode ser que esse recurso tenha que ser devolvido no futuro, mas essa informação só há como saber se houver uma verificação mais detalhada de recurso por recurso. Para verificar essas situações problemáticas em que o recurso não foi pago considerei no programa Python apenas valores liberados iguais a zero.

Partindo desse critério foi possível chegar aos primeiros resultados do desperdício. O arquivo total_perdas.csv mostra que em 20 anos as prefeituras perderam mais de 16 bilhões de reais. Recursos que já estavam garantidos, mas que não foram revertidos em benefícios para a população. Para complementar, também criei o arquivo total_inconsistente.csv que mostra o valor de recursos nestas situações e que aparecem com valor liberado diferente de zero. 

Refinando os dados: municípios

valores_convenios_por_municipio_1996_2017 valores_liberados_por_municipio_1996_2017 perdas_recursos_municipios_1996_2017 
Após apurar os dados gerais, esses três arquivos csv acima mostram os recursos verificados com base no número do município no SIAFI. Tive dificuldade em mostrar o nome do município e UF no próprio csv, por isso cruzei todos esses dados no SQLite. No SQLite também reuni informações sobre o cadastro dos moradores do município no programa Bolsa Família, a classificação no IDH (http://atlasbrasil.org.br/2013/pt/ranking/) e número de habitantes. Sobre o IDH, utilizei dados de 2010 porque não encontrei mais recentes. Esses dados são importantes para dar uma ideia sobre o panorama econômico, social e populacional do município. 

Todos esses dados reunidos deram origem ao arquivo dados_finais_1996_2017.csv. Se transformou em NAN porque novamente teve que ser feita a substituição dos valores nulos pelo NaN para permitir as análises. Este arquivo nos permite analisar o quadro de cada município e os resultados serão apresentados a seguir. Para ter acesso a informações rápidas de um convênio ou imprimir dados de um município de forma geral utilizei o programa casos_isolados_py. 

Cidade que mais recebeu recursos

Brasília, a capital federal, foi a campeã disparada no recebimento de recursos em convênios. Foram mais de 104 bilhões em valores conveniados e quase 50 bilhões liberados. A capital do Estado de São Paulo aparece em segundo lugar, mas seus números são bem distantes. A cidade teve 28 bilhões em recursos conveniados e 21 bilhões liberados. A coluna LiberadoHabitante também corrobora a liderança de Brasília no recebimento de recursos. Foram 16.287 reais liberados por habitante. 

Recursos por número de habitantes

Ainda analisando a relação recursos por número de habitantes, a cidade que ocupa a segunda posição não é uma capital, é Dourados, no Mato Grosso do Sul. No município, que possui cerca de 220 mil habitantes, foram liberados 11.650 reais por habitante. Classificando os dados por LiberadoHabitantes é possível conferir que as primeiras colocadas possuem bom IDH e baixa dependência do Bolsa Família, mas a cidade de Coronel João Sá, na Bahia, é uma exceção.  
     
Coronel João Sá possui cerca de 16 mil habitantes e fica a 470 quilômetros de Salvador. O município tem IDH baixo e 25% dos habitantes recebem auxílio do Bolsa Família. Apesar disso, a cidade está no top 10 dos maiores valores liberados por habitante, que chega a 7.796 reais. Com um índice muito baixo de perda de recursos, que não chega a 2%, o município teve poucos problemas com os convênios e consegue investir quase a totalidade do montante que vem sendo destinado.

Cidades que menos receberam recursos

Os cinco municípios que menos receberam recursos via convênios nos últimos 20 anos estão todos na região Sudeste e no mesmo estado: em Minas Gerais. Além de estarem localizados no mesmo estado, esses municípios têm outra característica em comum: todos têm IDH médio.
 
O município de Moeda fica a 60 quilômetros de Belo Horizonte e desde 1996 a cidade somou cerca de 435 mil reais em convênios. O último foi firmado em 2012 para auxiliar pessoas atingidas por desastres naturais, que podem ter sido as chuvas de 2012, que fizeram subir o nível do rio Paraopeba, deixando moradores desabrigados. Mas receber poucos recursos pode não ser sinônimo de precariedade. Com forte vocação turística a cidade possui estrutura hoteleira e restaurantes para atender aos turistas que buscam refúgio em um lugar tranquilo e cachoeiras na Serra da Moeda.

O segundo município que menos recebeu recursos é Casa Grande, localizado a 70 quilômetros de São João Del Rei. Até hoje a cidade só celebrou quatro convênios com o governo e perdeu mais da metade do valor total por problemas na tramitação. Em 20 anos, Casa Grande recebeu apenas 147 mil reais. 

O município de Albertina, terceiro colocado no ranking, teve seu último convênio firmado há quase 10 anos. A cidade fica próxima a Poços de Caldas e faz parte do circuito das malhas. As outras duas cidades mineiras que estão entre as menos favorecidas são Santana de Cataguases e Passa Vinte. 

Municípios que mais perderam recursos

No top 10 dos municípios que mais perderam recursos proporcionalmente ao valor total de convênios se destacam cidades do Amazonas, Goiás e Minas Gerais. Alvarães, que fica no Amazonas, lidera o ranking com uma perda definitiva de 72,5% do valor de convênios. No entanto, analisando os números com mais rigor, encontrei um convênio de 2010 de mais de 17 milhões de reais que seriam investidos na instalação de uma estrutura portuária de pequeno porte. Esse e outros convênios com a mesma finalidade feito com o DNIT em outros municípios amazonenses foram cancelados após a realização de auditorias do Tribunal de Contas da União, TCU, que comprovou superfaturamento nas obras. Segundo o TCU, bastariam pouco mais de cinco milhões para construir a instalação. Excluindo esse convênio do cálculo, o índice de perdas em Alvarães cai para 8%. 

As outras duas cidades amazonenses afetadas pelo mesmo problema foram Anori e Anamã. Em Anori o recurso destinado para a instalação portuária foi de mais de 10 milhões de reais. Excluindo esse convênio as perdas de Anori são mais elevadas em relação a Alvarães, 16%. Finalmente, em Anamã, o convênio do porto foi de pouco mais de 16 milhões de reais. Sem considerar esse recurso, as perdas chegariam a 6,7%. 

Essas cidades do interior do Amazonas têm no rio seu único acesso a outras localidades e as saídas dos barcos são bem precárias.

A segunda colocada no ranking é a cidade de Jandaia, em Goiás. O município conseguiu um convênio de quase 13 milhões de reais do programa Centro-Oeste: Desenvolvimento da Agricultura Irrigada para a construção de uma barragem. Pelas consultas feitas no portal Siconv, o sistema de cadastramento e acompanhamento de convênios do Governo Federal, uma provável causa para a anulação pode ter sido a extrapolação dos pedidos de prorrogação de prazo. Seria necessário consultar uma fonte para verificar.

O recurso de mais de seis milhões de reais para a implantação de um sistema para o tratamento do lixo chegou a ser anunciado no site da cidade vizinha de Anápolis (http://www.anapolis.go.gov.br/portal/multimidia/noticias/ver/consasup3rcio-da-apa-do-ribeirapo-joapo-leite-ac-novamente-pauta-de-reuniapo), que também faz parte da região chamada APA do Ribeirão João Leite.

O terceiro da lista com maiores registros de perdas de convênios é Casa Grande, em Minas Gerais, já citado no tópico dos municípios que menos receberam recursos. Em quarto lugar vem outra cidade goiana, Terezópolis de Goiás. Conforme parecer exarado pela FUNASA, a falta do cumprimento de obrigações dentro do prazo teria levado à extinção do convênio. Terezópolis de Goiás possui pouco mais de 2 mil habitantes e faz parte da APA do Ribeirão João Leite que reúne sete municípios. 

Novo Santo Antônio é uma cidade do Mato Grosso que fica às margens do Rio das Mortes ocupa o quinto lugar. O levantamento de seus convênios com o Python mostra que o município celebrou poucos convênios nos últimos 20 anos:

NOVO SANTO ANTONIO MATO GROSSO Data Publicação 2017-09-22 Órgão Superior MINISTERIO DAS CIDADES Situação do convênio Em Execucao Valor do convênio 295300.00 0.00
NOVO SANTO ANTONIO MATO GROSSO Data Publicação 2010-12-22 Órgão Superior MINISTERIO DA INTEGRACAO NACIONAL Situação do convênio Anulado Valor do convênio 5775521.37 0.00
NOVO SANTO ANTONIO MATO GROSSO Data Publicação 2014-05-15 Órgão Superior MINISTERIO DA SAUDE Situação do convênio Adimplente Valor do convênio 2316114.99 1672759.19
NOVO SANTO ANTONIO MATO GROSSO Data Publicação 2010-01-19 Órgão Superior MINISTERIO DA SAUDE Situação do convênio Adimplente Valor do convênio 500000.00 500000.00
NOVO SANTO ANTONIO MATO GROSSO Data Publicação 2008-06-09 Órgão Superior MINISTERIO DA EDUCACAO Situação do convênio Concluido Valor do convênio 112860.00 112860.00

Chama a atenção o valor de cinco milhões de reais de um convênio anulado com o Ministério da Integração Nacional. Em 2010, ano de início da vigência do contrato, já havia projeto básico cadastrado no Siconv, mas uma reportagem do G1 Mato Grosso de 2015 revela o que aconteceu com este convênio (http://g1.globo.com/mato-grosso/noticia/2015/03/devido-intensa-chuva-prefeito-de-cidade-de-mt-decreta-emergencia.html). 

O investimento para a construção de um muro de contenção para impedir o avanço do rio sobre a cidade em períodos de chuvas já estava garantido, mas com o assassinato do prefeito em 2011 o projeto parou e, como é possível conferir nas imagens, a obra está fazendo falta e a cheia continua causando muitos transtornos aos moradores.

Em seguida aparece Alumínio, localizada na região de Sorocaba, SP, que recebe este nome por conta da Companhia Brasileira do Alumínio, instalada no município. O maior convênio obtido por Alumínio foi de quase dois milhões de reais para a pavimentação de vias do bairro Jardim Olidel que ligariam esse local ao novo terminal rodoviário. No site do Siconv não há cadastro de projeto desta obra e nem pedidos de prorrogação de prazos. Publicado em 2010, o convênio foi anulado em 2012.

Cidade Vizinha da já mencionada Casa Grande, Queluzito, município mineiro de quase dois mil habitantes, chama a atenção por ter feito seis convênios com o Ministério das Cidades, sendo que nenhum deles evoluiu. Foram pouco mais de 900 mil reais em convênios para obras, como, por exemplo, pavimentação de ruas, construção de praça e de muro de arrimo. Quatro dos seis convênios em que foi possível checar a situação no site do Siconv, não havia projetos cadastrados.
  
No cadastro no Sinconv a prefeitura reforça a urgência da necessidade de melhorias em algumas ruas do município, inclusive na rua do Correio. O campo “Capacidade Técnica e Gerencial está em branco o que indica a falta de profissionais qualificados para dar andamento aos projetos do Ministério das Cidades. O município acaba perdendo o recurso e ainda pode estar tirando a chance de outra cidade que tenha condições de receber o investimento. 
 
A rua do Correio na verdade se chama Professor Manoel Lino, portanto o cadastro no Siconv foi feito com o apelido da via, o que demonstra a falta de seriedade e compromisso com o recurso.

Fechando o top 10 das cidades que mais perdem recursos, aparece outra cidade mineira. Carbonita, localizada no Alto Jequitinhonha, teve um problema semelhante ao do município de Terezópolis de Goiás. Mais de seis milhões de reais seriam liberados pela FUNASA para a implantação de uma unidade de tratamento de resíduos sólidos. Entre as explicações para o não prosseguimento do convênio, a FUNASA esclarece que o município já possui uma unidade similar de tratamento do lixo e não houve um crescimento populacional que justifica a implantação de um segundo sistema. Além disso, ao final do parecer, o órgão também ressalta a falta de interesse do município.

Bons exemplos: cidades que investem

O Brasil possui 5570 municípios, incluindo Fernando de Noronha, que na verdade não é uma cidade, é um distrito estadual, mas esta região de Pernambuco não possui código no SIAFI e não faz convênios com o Governo Federal. Portanto, de 5569 cidades há um seleto grupo de 183 municípios que não apresentam problemas com seus convênios e tudo indica que tudo está indo bem, pelo menos por enquanto.
Ermo, uma cidade catarinense que tem apenas 24 anos de emancipação do município de Turvo, faz convênios com o Governo Federal desde 2003. Um print no Python mostra que não há qualquer tipo de pendência com o governo, não há nenhum projeto em andamento e todos os recursos destinados até hoje foram integralmente utilizados. Os valores conveniado e pago são exatamente iguais: R$ 2.193.456,10.

ERMO | SANTA CATARINA | Órgão Superior MINISTERIO DAS CIDADES | Situação do convênio Em Execucao | Valor do convênio 245850.00 | Valor Liberado 245850.00
ERMO | SANTA CATARINA | Órgão Superior MINIST. DA AGRICUL..PECUARIA E ABASTECIMENTO | Situação do convênio Prestacao de Contas Aprovada | Valor do convênio 146250.00 | Valor Liberado 146250.00
ERMO | SANTA CATARINA | Órgão Superior MINIST. DA AGRICUL..PECUARIA E ABASTECIMENTO | Situação do convênio Prestacao de Contas Aprovada | Valor do convênio 97500.00 | Valor Liberado 97500.00
ERMO | SANTA CATARINA | Órgão Superior MINIST. DA AGRICUL..PECUARIA E ABASTECIMENTO | Situação do convênio Prestacao de Contas Aprovada | Valor do convênio 224250.00 | Valor Liberado 224250.00
ERMO | SANTA CATARINA | Órgão Superior MINIST. DA AGRICUL..PECUARIA E ABASTECIMENTO | Situação do convênio Prestacao de Contas Aprovada | Valor do convênio 156000.00 | Valor Liberado 156000.00
ERMO | SANTA CATARINA | Órgão Superior MINISTERIO DO DESENVOLVIMENTO AGRARIO | Situação do convênio Prestacao de Contas Aprovada | Valor do convênio 100000.00 | Valor Liberado 100000.00
ERMO | SANTA CATARINA | Órgão Superior MINISTERIO DO DESENVOLVIMENTO SOCIAL | Situação do convênio Prestacao de Contas Aprovada | Valor do convênio 100000.00 | Valor Liberado 100000.00
ERMO | SANTA CATARINA | Órgão Superior MINISTERIO DO ESPORTE | Situação do convênio Prestacao de Contas Aprovada | Valor do convênio 243750.00 | Valor Liberado 243750.00
ERMO | SANTA CATARINA | Órgão Superior MINISTERIO DAS CIDADES | Situação do convênio Concluido | Valor do convênio 98200.00 | Valor Liberado 98200.00
ERMO | SANTA CATARINA | Órgão Superior MINISTERIO DAS CIDADES | Situação do convênio Concluido | Valor do convênio 78420.00 | Valor Liberado 78420.00
ERMO | SANTA CATARINA | Órgão Superior MINISTERIO DA SAUDE | Situação do convênio Concluido | Valor do convênio 63000.00 | Valor Liberado 63000.00
ERMO | SANTA CATARINA | Órgão Superior MINISTERIO DA SAUDE | Situação do convênio Concluido | Valor do convênio 40000.00 | Valor Liberado 40000.00
ERMO | SANTA CATARINA | Órgão Superior MINIST. DA AGRICUL..PECUARIA E ABASTECIMENTO | Situação do convênio Concluido | Valor do convênio 68250.00 | Valor Liberado 68250.00
ERMO | SANTA CATARINA | Órgão Superior MINISTERIO DAS CIDADES | Situação do convênio Concluido | Valor do convênio 236466.10 | Valor Liberado 236466.10
ERMO | SANTA CATARINA | Órgão Superior MINISTERIO DAS CIDADES | Situação do convênio Concluido | Valor do convênio 50000.00 | Valor Liberado 50000.00
ERMO | SANTA CATARINA | Órgão Superior MINISTERIO DAS CIDADES | Situação do convênio Concluido | Valor do convênio 80000.00 | Valor Liberado 80000.00
ERMO | SANTA CATARINA | Órgão Superior MINISTERIO DA SAUDE | Situação do convênio Concluido | Valor do convênio 40000.00 | Valor Liberado 40000.00
ERMO | SANTA CATARINA | Órgão Superior MINISTERIO DA INTEGRACAO NACIONAL | Situação do convênio Concluido | Valor do convênio 120000.00 | Valor Liberado 120000.00
ERMO | SANTA CATARINA | Órgão Superior MINISTERIO DA EDUCACAO | Situação do convênio Concluido | Valor do convênio 3520.00 | Valor Liberado 3520.00
ERMO | SANTA CATARINA | Órgão Superior MINISTERIO DA EDUCACAO | Situação do convênio Concluido | Valor do convênio 2000.00 | Valor Liberado 2000.00
 
No Siconv é o próprio prefeito que indica o responsável técnico, neste caso, pela pavimentação de vias do município

Neste grupo de cidades que investem bem o dinheiro, existem quatro que não apresentam índice de desenvolvimento humano, IDH. Isso porque para este estudo foi utilizado o balanço de 2010 e essas cidades foram emancipadas ou na mesma época ou após, sendo elas Paraíso das Águas, MS, Mojuí dos Campos, PA, Pescaria Brava e Balneário Rincão, ambas em Santa Catarina.

Ainda falando sobre o seleto grupo de 183 municípios, constatei que entre eles não há nenhum com IDH classificado como Muito Alto ou Muito Baixo. Já com as classificações Alto e Médio há diversos e isso já era esperado. Mas será que há cidades com IDH Baixo, com significativa dependência do programa Bolsa Família que aproveitam bem os recursos dos convênios? Para descobrir fiz um novo programa (pesquisar_melhores_cidades.py) e filtrei por municípios que possuem relação de número de habitantes por beneficiários do Bolsa Família de mais de 20%, IDH Baixo, perda definitiva e valor inconsistente (aquele que aparece como excluído ou rescindido, mas que apresenta valores pagos e os concluídos que têm valores liberados iguais a zero) nulos. O Python gerou essa lista:

GEMINIANO PIAUI NaN-55 21
AROEIRAS DO ITAIM PIAUI NaN-55 22

Geminiano possui pouco mais de 3,5 mil habitantes, sendo que 21% deles recebem Bolsa Família, e fica localizada próxima à divisa com o Ceará. Até hoje o município celebrou 33 convênios com o Governo Federal e tem um alto índice de aproveitamento dos recursos. Teve um convênio arquivado e está inadimplente em outro. No restante ou o convênio já foi concluído ou está em execução. 
É possível que na grande lista de convênios tenham outros bons exemplos de gestão pública, mas aqui selecionei este exemplo para mostrar que não é porque o município não tem condições favoráveis que ele deixa de investir.

Aroeiras do Itaim, que assim como Geminiano está localizada na região da cidade de Picos, no Piauí, firmou convênios até hoje que totalizam pouco mais de 2,7 milhões de reais. Mas estudando melhor os dados, dá para notar que a situação não é tão boa quanto aparenta. Dois convênios com a FUNASA de 750 mil reais e de 400 mil reais são para a realização de melhorias habitacionais para combater a proliferação do chamado bicho barbeiro, transmissor da doença de chagas, nas localidades de Carnaibinha, Chapada, Fazenda Nova e Tinguis. Porém, segundo relatório do TCU, o primeiro convênio de 750 mil reais, do Programa de Aceleração do Crescimento, teve problemas. O município recebeu até o momento 40% do recurso, mas concluiu apenas 10% das melhorias e corre o risco de ter que devolver o dinheiro corrigido à União. 

São comuns os casos de municípios que devolvem dinheiro ao Governo Federal. Este caso da construção da Unidade de Pronto Atendimento, UPA, de Brusque, Santa Catarina, é um exemplo (https://omunicipio.com.br/prefeitura-devolvera-dinheiro-da-construcao-do-predio-da-upa-24-horas/). A prefeitura iniciou a obra e só depois fez os cálculos de manutenção. Com a conclusão que a cidade não teria recursos para isso, a saída foi devolver o dinheiro. 

Convênios com o exterior

Não é só com municípios brasileiros que o governo estabelece os convênios. Eles também beneficiam outros países. Desde 1996 o Brasil já fez convênios na ordem de mais de 270 milhões de reais, sendo que a maior parte, 260 milhões, já foram liberados. Fazendo um ranking dos municípios incluindo o exterior, esta classificação ocupa a 68ª posição, ou seja, fica a frente da maior parte das cidades brasileiras. Para se ter uma ideia atrás do exterior estão municípios com mais de 500 mil habitantes: Jaboatão dos Guararapes, PE, Ananindeua, PA, Joinville, SC, São Gonçalo, RJ, Serra, ES e Sorocaba, SP.

As distorções também acontecem entre os municípios que receberam mais recursos que o exterior. Viçosa, em Minas Gerais, tem pouco mais de 78 mil habitantes e recebeu até hoje mais de 350 milhões de reais. Ipojuca, no Pernambuco, famosa pela praia de Porto de Galinhas, com pouco mais de 94 mil habitantes recebeu mais de 270 milhões. Iperó, localizada no interior de São Paulo, que possui cerca de 35 mil habitantes, já somou 190 milhões em valores liberados. Grande parte destes recursos foi destinada ao Programa Nuclear da Marinha, localizado no município. 

Utilizei o programa exterior.py para imprimir esses resultados da população e conferir com o csv. Infelizmente fiz essa comparação manualmente por falta de conhecimento para aprimorar o programa. 

Quanto aos convênios no exterior, o que apresentou maior liberação foi um acordo para a divulgação da língua portuguesa na Colômbia que recebeu mais de 216 milhões de reais. Todos os convênios do Brasil com o exterior podem ser consultados no arquivo exterior_convenios_1996_2017.csv.

Conclusões

Este levantamento comprovou que o descaso das prefeituras com o dinheiro público, salvo exceções apresentadas no estudo, ainda impera, prejudicando o acesso da população a serviços públicos e privando os cidadãos de viverem melhor no município. Convênios que envolvem apenas aquisição de bens, como ônibus, máquinas agrícolas, equipamentos hospitalares, entre outros, geralmente têm andamento, ao contrário de demandas de construção, reforma e outras ações que exigem projetos alinhados com os critérios da Caixa Econômica Federal ou de outro órgão que concede o valor. Estes casos, sem dúvida, são os mais dramáticos porque podem culminar com situações extremas em que o município não reverte o benefício para a população e ainda precisa devolver o dinheiro, adquirindo mais uma dívida. 

Seria muito importante que a população acompanhasse o Portal da Transparência e tivesse essas informações de como o investimento é tratado pelo seu município, cobrando seriedade e compromisso do prefeito ou da prefeita. Essa ferramenta é um ótimo caminho para selecionar pessoas corretas e elevar o nível do serviço público e da própria atividade política nos municípios.

É importante esclarecer que há muitos dados inconsistentes no Portal da Transparência que geram muitas dúvidas. Cidades que tiveram convênio excluído, mas apresentam valor pago. Há cidades cuja situação está como concluída, mas o valor liberado é zero. Enfim, pode ser erro de cadastramento ou algum outro fator que desconhecemos, por isso foram criadas as colunas de inconsistências no arquivo dados_finais_1996_2017.csv para mostrar essas dúvidas. Essas inconsistências prejudicam o trabalho de apuração. 

Quanto às ferramentas de pesquisa, a linguagem Python foi uma grande facilitadora. Proporciona acesso rápido a alguma informação necessária para o momento e permite lidar com uma grande quantidade de dados, neste caso um txt com mais de 400 mil linhas, viabilizando a exploração deste tipo de arquivo. Sem um sistema automatizado, essa busca levaria meses ou anos.

O foco deste projeto foram os municípios e, mesmo entre eles, ainda há diversos dados que podem ser explorados. Este trabalho foi apenas uma ponta das muitas possibilidades que existem para chegar a dados muito relevantes sobre as nossas cidades.     
