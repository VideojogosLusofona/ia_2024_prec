<!--
Projeto de Recurso de Inteligência Artificial 2024/25 (c) by Nuno Fachada

Projeto de Recurso de Inteligência Artificial 2024/25 is licensed under a
Creative Commons Attribution-NonCommercial-ShareAlike 4.0 International License.

You should have received a copy of the license along with this
work. If not, see <http://creativecommons.org/licenses/by-nc-sa/4.0/>.
-->

# Projeto de Recurso de Inteligência Artificial 2024/25

## Introdução

Os grupos devem implementar, em Unity 6000.0, um clone do jogo clássico [Lunar
Lander] baseado no projeto inicial incluído neste repositório.

O projetos têm de ser desenvolvidos por **grupos de 2 a 3 alunos** (não são
permitidos grupos individuais). Até **16 de junho** é necessário que:

* Indiquem a composição do grupo.
* Clonem este repositório para um repositório **privado** no Github.
* Indiquem o URL do repositório **privado** do vosso projeto no GitHub.
* Convidem o [docente][Nuno Fachada] para ter acesso a esse repositório privado.

Os projetos serão automaticamente clonados às **23h00 de 22 de junho** sendo a
entrega feita desta forma, sem intervenção dos alunos. Repositórios e/ou
projetos não funcionais nesta data não serão avaliados.

## O projeto incluído neste repositório

O projeto incluído neste repositório consiste numa versão muito preliminar do
[Lunar Lander] no qual é possível controlar o módulo lunar usando as teclas
&#8592; (rodar para a esquerda), &#8593; (ligar propulsores), &#8594; (rodar
para a direita). Não está implementado qualquer cenário nem colisões.

Embora seja possível mudar algumas constantes (gravidade, grau de rotação,
força aplicada durante a propulsão), é necessário manter os seguintes
princípios inalterados:

* O módulo lunar está sujeito a física (isto é, a movimento dinâmico).
* A propulsão implica a aplicação de uma força ao módulo lunar no sentido para
  o qual ele está virado.
  * É possível limitar a velocidade máxima do módulo se isso for conveniente.
* A rotação é aplicada diretamente, não estando sujeita a forças (isto é,
  movimento cinemático).

Para terem uma ideia da versão original do jogo, podem jogar a versão muito
semelhante [neste link](https://arcader.com/lunar-lander/).

## Componentes do projeto

O projeto tem duas componentes, com igual peso na nota: 1) uma implementação em
Unity; e, 2) o relatório. O projeto e o relatório (ficheiro `README.md` a
colocar na raiz do projeto) devem **obrigatoriamente** ser desenvolvidos com
Git. A autoria dos _commits_ será tida em conta na nota final, sendo aceitável
que algum elemento do grupo se dedique maioritariamente ao relatório. Não se
esqueçam de incluir os ficheiros [`.gitignore`] e [`.gitattributes`] adequados na
raiz no projeto, bem como de utilizar Git LFS logo de início.

Em ambas as componentes, são preferíveis trabalhos mais pequenos e limitados,
mas bem estruturados e apresentados, do que trabalhos enormes com _bugs_, pouco
funcionais e mal escritos. Privilegiem qualidade e não quantidade.

### Implementação em Unity

#### Os básicos

* O _lander_ deve ter uma quantidade limitada de combustível, que diminui quando
  os propulsores estão ligados.
* Quando o combustível atinge zero, os propulsores deixam de funcionar e o
  módulo lunar fica sujeito apenas à força da gravidade (embora a rotação ainda
  funcione).
* Cada cenário deve ser gerado procedimentalmente (ver próxima secção) e ter
  apenas um local de aterragem horizontal com o dobro da dimensão horizontal do
  _lander_.
* O UI deve indicar a quantidade de combustível disponível, as velocidades
  horizontais e verticais do módulo em cada momento, bem como a distância até ao
  centro do local de aterragem.
* Cada sessão/cenário tem apenas dois desfechos: sucesso ou insucesso. Uma
  aterragem de sucesso exige que o módulo aterre na zona de aterragem com
  rotação zero e velocidade geral (`rb.velocity.magnitude`) abaixo de um valor
  bastante pequeno (a definir pelos estudantes). Caso contrário o _lander_ é
  destruído e o jogo termina em insucesso.

#### Geração procedimental de conteúdos

* Os cenários de aterragem devem ser gerados procedimentalmente, usando por
  exemplo o algoritmo de _midpoint displacement_ dado nas aulas (código no
  Moodle). A zona de aterragem horizontal deve ser colocada aleatoriamente numa
  parte do cenário e deve ser claramente marcada com uma cor diferente.
* O céu estrelado deve ser gerado usando um _quasi-random number generator_,
  como por exemplo o gerador de Halton ou Poisson Disks.
* Podem usar outras alternativas tanto para o cenários de aterragem, como para o
  céu, desde que bem justificadas e referenciadas.

#### _Polish_

Antes de passarem para a parte de aprendizagem, devem polir muito bem o vosso
protótipo, sendo as aterragens desafiantes mas não muito difíceis para o jogador
humano. Após chegarem a um conjunto interessante de parâmetros, não os
modifiquem mais. Caso contrário poderão ter de iterar várias vezes a fase de
aprendizagem automática, complicando e atrasando a finalização deste projeto.

#### Aprendizagem automática

Esta é a parte principal do projeto. Em primeiro lugar, convém definir
claramente os _inputs_ e _outputs_ do problema. Fica uma sugestão:

* _**Inputs**_ (estado do jogo):
  * Quantidade de combustível
  * Velocidade horizontal
  * Velocidade vertical
  * Rotação atual
  * Distância horizontal ao centro da zona de aterragem
  * Distância vertical em linha reta ao chão (seja zona de aterragem ou não)
* _**Outputs**_ (ação a aplicar dado o estado do jogo):
  * Rotação (cinemática): esquerda, nenhuma, direita
  * Propulsão (dinâmica, força constante): ligada, desligada

Tendo em conta estas entradas e saídas, os estudantes devem implementar um ou
mais algoritmos de _machine learning_ que aprendem a jogar jogando múltiplas
vezes em _fast forward_, modificando o `Time.timeScale`. Algoritmos sugeridos:

* Hill climber e suas variações
* Simulated annealing
* Algoritmos genéticos (avançado)
* Particle swarm optimization (avançado)
* Reinforcement learning (muito avançado, se optarem por esta abordagem sugiro o
  uso do [Unity ML-Agents][ml-agents])

No entanto, coloca-se a questão de que parâmetros otimizar. Primeiro, é
necessário criar uma função para relacionar os _outputs_ com os _inputs_
(possivelmente relacionar cada _output_ separadamente com os _inputs_). Podem
tentar criar esta função (funções) manualmente, procurar na Internet, discutir
abordagens com IAs generativas (e.g., ChatGPT), etc.

Outra possibilidade para criar esta função consiste em implementar uma rede
neuronal simples, com uma ou duas camadas escondidas, pesos inicialmente
aleatórios e que vão sendo otimizados por um dos algoritmos previamente
sugeridos. Esta é uma abordagem mais avançada, e provavelmente já precisa de
algoritmos genéticos ou particle swarms para fazer uma otimização mais
eficiente.

Em qualquer uma das abordagens é necessário ter uma função de avaliação
(_fitness_), que indica a qualidade de cada uma das soluções. Esta função terá
valor máximo para aterragens perfeitas.

A ideia é que os estudantes explorem, estudem, e experimentem as várias
possibilidades, sendo que discussões com colegas, professores, e mesmo ChatGPT e
afins esperadas para que cheguem a uma boa solução final.

#### Menu principal e fluxo do programa

O menu deve ter pelo menos quatro opções:

* _**Human Game**_: _lander_ controlado pelo jogador humano (a IA **não** está
  a aprender nada, isto serve apenas para os estudantes e professor jogarem
  ao jogo).
* _**AI Game**_: _lander_ controlado pela IA treinada (inicialmente não está
  treinada e vai jogar pessimamente dada a função não otimizada que
  implementaram).
* _**AI Training**_: Esta opção serve para treinar a IA com o algoritmo
  escolhido. Se testarem vários algoritmos, poderão ter várias opções, uma
  para cada algoritmo diferente. É expectável que o treino demore longos
  minutos, mesmo com o `timeScale` no máximo.
* _**Quit**_: Sair da aplicação

#### Notas adicionais

Para implementação deste projeto é crucial entender bem as partes relevantes de
PCG e _machine learning_. A discussão do projeto incidirá sobretudo sobre
aspetos específicos das opções que tomaram.

Se tiverem dúvidas sobre como proceder em alguma fase do projeto, entrem em
contacto (pelo menos uma semana antes do prazo de entrega).

#### Código e Git

O código do projeto, bem como o uso de Git, devem seguir as melhores práticas
lecionadas em LP1 e LP2 e descritas nos diferentes enunciados dessas
disciplinas.

Estas componentes não são o foco principal desta avaliação, mas projetos mal
programados e com fraco uso de Git serão penalizados.

Todos os membros do grupo devem contribuir para o repositório, de preferência
tanto para o código como para o relatório.

### Relatório

O relatório deve estar incluído no repositório em formato [Markdown] (ficheiro
`README.md`, ou seja, este ficheiro, que podem editar à vossa medida), e deve
estar organizado da seguinte forma:

Deve também ser realizado um relatório com a seguinte estrutura:

1. **Título do projeto**
2. **Nome dos autores** (primeiro e último) e respetivos **números de aluno**
3. **Informação de quem fez o quê no projeto**
   - Esta informação é obrigatória e será cruzada com os _commits_ efetuados. A
     não indicação desta informação implica a não avaliação do projeto
     (equivalente a nota zero).
4. **Introdução**
   - Pequena descrição sobre o problema e a forma como o resolveram. Deve
     oferecer ao leitor informação suficiente para entender e contextualizar o
     projeto, bem como quais foram os objetivos e resultados alcançados.
   - Deve também ser apresentada nesta secção a pesquisa efetuada sobre _machine
     learning_ em jogos simples como o [Lunar Lander]. Devem pesquisar por
     artigos científicos concretos no [Google Scholar] e analisar pelo menos dois
     deles em profundidade. Os artigos devem ser diretamente relacionados com
     este tópico, e eventualmente (mas não necessariamente) inspirar a vossa
     solução.
5. **Metodologia**
   - Explicação de como a solução foi implementada, que algoritmos foram usados,
     valores parametrizáveis, figuras explicativas (por exemplo diagramas UML
     simples ou fluxogramas de algum algoritmo mais complexo que tenham
     desenvolvido.
   - Esta secção deve ter detalhe suficiente para que outra pessoa consiga
     replicar o comportamento do vosso projeto sem olhar para o respetivo código.
6. **Resultados e discussão**
   - Apresentação dos resultados, salientando os aspetos mais interessantes que
     observaram no treino do agente (da nave), dificuldades encontradas, etc.
   - Caso tenham experimentado diferentes parâmetros dos algoritmos utilizados,
    podem apresentar quadros, tabelas e/ou gráficos com informação que
    considerem importante (nº de iterações de treino, percentagem de sucessos,
    etc).
   - Na parte da discussão, devem fazer uma interpretação dos resultados que
     observaram, realçando quaisquer correlações que tenham encontrado entre
     estes e as parametrizações/algoritmos que utilizaram, bem como resultados
     inesperados, propondo hipóteses explicativas.
7. **Conclusões**
   - Nesta secção devem relacionar o que foi apresentado na introdução,
     nomeadamente o problema que se propuseram a resolver, com os resultados que
     obtiveram, e como o vosso projeto e a vossa abordagem se relaciona no
     panorama geral da pesquisa que efetuaram sobre _machine learning_ em jogos
     simples deste tipo.
   - Uma pessoa que leia a introdução e conclusão do vosso relatório deve ficar
     com uma boa ideia daquilo que fizeram e descobriram, embora sem saber os
     detalhes.
8. **Agradecimentos**
   - Nesta secção devem agradecer às pessoas externas ao grupo que vos ajudaram
     no projeto, caso tenha sido esse o caso.
9. **Referências**
   - Indicação de todas as referências utilizadas, sejam artigos (científicos ou
     não), livros, repositórios de código, sites, blogs, vídeos, etc.
   - As referências devem estar em formato [APA], sendo as citações efetuadas ao
     longo do relatório no momento em que falam do respetivo assunto.

Todas as secções são obrigatórias, exceto os agradecimentos caso não tenham
tido ajuda de terceiros. O relatório pode ser escrito em Português ou Inglês,
sendo bonificados relatórios curtos, concisos e bem apresentados. O relatório
será avaliado com base na inclusão e qualidade dos pontos pedidos no enunciado,
na qualidade na escrita (os erros ortográficos serão penalizados), na
formatação Markdown, e na formatação correta das referências em estilo APA.

 ## Grupos e avaliação

Os grupos são de 2 ou, preferencialmente, de 3 alunos. Este projeto tem um peso
de 50% na nota final da disciplina. A implementação em Unity e o relatório
terão igual peso (50%) na nota do projeto. A nota individual de cada membro do
grupo dependerá da respetiva qualidade e quantidade do trabalho efetuado,
analisada com base no descrito no relatório, nos _commits_ do Git e na
discussão. Como já referido, se tiverem forçosamente de fazer uma escolha, é
preferível optarem pela qualidade em detrimento da quantidade.

## Entrega

O projeto é entregue de forma automática através do GitHub. Mais concretamente,
o repositório do projeto será automaticamente clonado às **23h00 de 22 de
junho de 2025**. Certifiquem-se de que a aplicação está funcional e que todos os
requisitos foram cumpridos, caso contrário o projeto não será avaliado.

O repositório deve ter:

* Projeto Unity funcional segundo os requisitos indicados.
* Ficheiros `.gitignore` e `.gitattributes` adequados para projetos Unity
  (já estão incluídos neste repositório).
* Ficheiro `README.md` contendo o relatório do projeto em formato [Markdown]
  (podem editar diretamente este ficheiro após fazerem o _clone_ deste
  repositório).
* Ficheiros de imagens, contendo os diagramas figuras que considerem úteis.
  Estes ficheiros devem ser incluídos no repositório em modo Git LFS (assim como
  todos os _assets_ binários do Unity).

Em nenhuma circunstância o repositório pode ter _builds_ ou outros ficheiros
temporários do Unity (que são automaticamente ignorados se usarem um
`.gitignore` apropriado).

## Honestidade académica

Nesta disciplina, espera-se que cada aluno siga os mais altos padrões de
honestidade académica. Isto significa que cada ideia que não seja do
aluno deve ser claramente indicada, com devida referência ao respetivo
autor. O não cumprimento desta regra constitui plágio.

O plágio inclui a utilização de ideias, código ou conjuntos de soluções
de outros alunos ou indivíduos, ou quaisquer outras fontes para além
dos textos de apoio à disciplina, sem dar o respetivo crédito a essas
fontes. Os alunos são encorajados a discutir os problemas com outros
alunos e devem mencionar essa discussão quando submetem os projetos.
Essa menção **não** influenciará a nota. Os alunos não deverão, no
entanto, copiar códigos, documentação e relatórios de outros alunos, ou dar os
seus próprios códigos, documentação e relatórios a outros em qualquer
circunstância. De facto, não devem sequer deixar códigos, documentação e
relatórios em computadores de uso partilhado.

**Sobre IAs generativas (e.g. ChatGPT):** Podem usar este tipo de ferramentas
para esclarecer dúvidas, ou até para obter sugestões de código e/ou organização
do projeto, desde de que as ideias e organização geral do mesmo sejam originais.
Podem também usar estas ferramentas para corrigir o Português (ou Inglês) do
relatório. Código, arquiteturas de projeto, e relatórios completamente escritos
por uma IA generativa serão facilmente detetáveis, pelo que sugerimos muito
cuidado no uso deste tipo de ferramentas. De qualquer forma, toda a utilização
de IA generativa deve ser indicada em forma de comentários no código e nas
explicitamente indicada no relatório.

Nesta disciplina, a desonestidade académica é considerada fraude, com
todas as consequências legais que daí advêm. Qualquer fraude terá como
consequência imediata a anulação dos projetos de todos os alunos envolvidos
(incluindo os que possibilitaram a ocorrência). Qualquer suspeita de
desonestidade académica será relatada aos órgãos superiores da escola
para possível instauração de um processo disciplinar. Este poderá
resultar em reprovação à disciplina, reprovação de ano ou mesmo suspensão
temporária ou definitiva da Universidade Lusófona.

*Texto adaptado da disciplina de [Algoritmos e
Estruturas de Dados][aed] do [Instituto Superior Técnico][ist]*

## Referências

* Millington, I. (2019). AI for Games (3rd ed.). CRC Press.

## Licenças

* Este enunciado é disponibilizado através da licença [CC BY-NC-SA 4.0].
* O código que acompanha este enunciado é disponibilizado através da licença
  [MPL 2.0].

## Metadados

* Autor: [Nuno Fachada]
* Curso:  [Licenciatura em Videojogos][lamv]
* Instituição: [Universidade Lusófona - Centro Universitário de Lisboa][ULHT]

[CC BY-NC-SA 4.0]:https://creativecommons.org/licenses/by-nc-sa/4.0/
[MPL 2.0]:https://www.mozilla.org/en-US/MPL/2.0/
[lamv]:https://www.ulusofona.pt/licenciatura/videojogos
[Nuno Fachada]:https://github.com/nunofachada
[ULHT]:https://www.ulusofona.pt/
[aed]:https://fenix.tecnico.ulisboa.pt/disciplinas/AED-2/2009-2010/2-semestre/honestidade-academica
[ist]:https://tecnico.ulisboa.pt/pt/
[Markdown]:https://guides.github.com/features/mastering-markdown/
[Lunar Lander]:https://en.wikipedia.org/wiki/Lunar_Lander_(1979_video_game)
[ml-agents]:https://github.com/Unity-Technologies/ml-agents
[`.gitignore`]:https://github.com/VideojogosLusofona/unity_git/blob/master/.gitignore
[`.gitattributes`]:https://github.com/VideojogosLusofona/unity_git/blob/master/.gitattributes
[Google Scholar]:https://scholar.google.com/
[APA]:https://www.editage.com/insights/apa-style-cheat-sheet-how-to-cite-a-journal-article-using-apa-style