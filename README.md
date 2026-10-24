# Immunotherapies and LabTools in the Era of Precision Medicine 2026

# Roteiro de aula prática — Bioinformática estrutural aplicada a anticorpos

**Tema:** da sequência do anticorpo à análise da interface e ao design *de novo*  
**Formato:** atividade inteiramente online; os arquivos PDB/FASTA são baixados e enviados às ferramentas web, sem instalação de programas.  
**Duração sugerida:** 3–4 horas, ou duas aulas de aproximadamente 2 horas.  
**Nível:** graduação avançada/pós-graduação em Biologia, Biotecnologia, Bioinformática ou áreas afins.

## Objetivos de aprendizagem

Ao final da atividade, o aluno deverá ser capaz de:

1. obter e interpretar sequências variáveis de anticorpos e gerar um modelo estrutural;
2. prever regiões candidatas a epítopos e reconhecer limitações dessas previsões;
3. executar docking proteína–proteína e inspecionar os modelos de complexo;
4. interpretar estimativas computacionais de energia de ligação sem confundi-las com afinidade experimental;
5. examinar características associadas à desenvolvibilidade de anticorpos e compreender o objetivo do design de novo;
6. distinguir uma hipótese computacional de uma conclusão que exigiria validação experimental.

---

## Sistema-modelo escolhido

Usaremos a **lisozima da clara do ovo de galinha** (*hen egg-white lysozyme*, HEL) nos dois estudos de caso.

- **Estrutura de referência do complexo anticorpo–antígeno:** [PDB 1YQV](https://www.rcsb.org/structure/1YQV), complexo do fragmento Fab do anticorpo murino HyHEL-5 com lisozima, resolvido por cristalografia a 1,70 Å.
- **Estrutura isolada do antígeno para o estudo de design de novo:** [PDB 7S2Q](https://www.rcsb.org/structure/7S2Q), lisozima sem o anticorpo presente, com uma cadeia proteica de 129 resíduos e resolução de 1,60 Å.

O HyHEL-5 é um **controle positivo didático**: sua estrutura experimental permite comparar a pose prevista com um complexo conhecido. Para simular a etapa inicial de um projeto de descoberta, apresente a identidade do anticorpo aos alunos somente depois da primeira análise, se isso fizer sentido para a turma. No segundo caso, use a lisozima isolada (7S2Q) como entrada do RFantibody e mantenha 1YQV apenas como referência para uma discussão posterior.

> **Nota didática:** a lisozima é um alvo conveniente para ensinar o fluxo de trabalho, não um alvo biologicamente novo. O segundo caso é um exercício de design computacional contra um alvo conhecido, e não uma alegação de descoberta inédita.

## Ferramentas online

| Etapa | Ferramenta | Finalidade |
|---|---|---|
| Modelagem de anticorpo | [ImmuneBuilder — Tamarind Bio](https://app.tamarind.bio/tools/immunebuilder) | Predizer estrutura de VH e VL a partir das sequências variáveis |
| Predição de epítopos | [BepiPred-3.0 — servidor web da DTU](https://services.healthtech.dtu.dk/services/BepiPred-3.0/) | Estimar resíduos candidatos a epítopos de células B a partir da sequência do antígeno |
| Docking | [HDOCK](http://hdock.phys.hust.edu.cn/) | Gerar poses de interação entre antígeno e anticorpo |
| Visualização de estruturas | [RCSB Mol* 3D Viewer](https://www.rcsb.org/3d-view) | Visualizar PDBs locais e estruturas depositadas no PDB |
| Energia de ligação estimada | [MM/GB(PB)SA — Tamarind Bio](https://app.tamarind.bio/tools/gbsa) | Estimar energia de ligação a partir de um complexo proteico |
| Perfil de desenvolvibilidade | [TAP2 — Therapeutic Antibody Profiler](https://app.tamarind.bio/tools/tap) | Avaliar características relacionadas ao perfil de anticorpos terapêuticos |
| Humanização | [BioPhi — Tamarind Bio](https://app.tamarind.bio/tools/biophi) | Avaliar a sequência e explorar recomendações de humanização |
| Design *de novo* | [RFantibody — Tamarind Bio](https://app.tamarind.bio/tools/rfantibody) | Gerar modelos e sequências candidatas de anticorpos contra um antígeno |

**BepiPred-3.0 não precisa ser instalado no Colab para esta aula:** existe um servidor oficial que aceita FASTA e devolve gráficos interativos, resultados CSV e sequências de resíduos previstos. O repositório no GitHub pode ser usado em outra atividade, caso a turma queira instalar a versão local: [GitHub — BepiPred-3.0](https://github.com/UberClifford/BepiPred-3.0).

> A disponibilidade, os tempos de execução, a exigência de login e eventuais limites de uso das ferramentas online podem mudar. Faça um teste curto antes da aula e salve os arquivos de entrada/saída necessários.

---

# Estudo de caso 1 — Mapeamento de interações anticorpo–antígeno

## Enunciado para os alunos

> Você selecionou anticorpos por *phage display* e biopanning contra um alvo de interesse e sequenciou as regiões variáveis. Agora precisa investigar como um anticorpo se estrutura, onde pode se ligar ao antígeno e quais aspectos merecem avaliação antes de considerar uma aplicação terapêutica.

Neste exercício, as sequências variáveis do HyHEL-5 serão usadas como exemplo positivo. O objetivo é praticar o fluxo de análise e comparar as previsões com uma estrutura experimental conhecida.

## Arquivos e sequências de entrada

### 1. Sequências variáveis de anticorpo

As sequências abaixo correspondem às regiões variáveis de cadeia pesada (VH) e leve (VL), respectivamente. Elas foram separadas das cadeias completas de anticorpo no PDB 1YQV; **não** incluem as regiões constantes do Fab.

**VH — cadeia pesada variável**

```fasta
>HyHEL5_VH
EVQLQQSGAELMKPGASVKISCKASGYTFSDYWIEWVKQRPGHGLEWIGEILPGSGSTNYHERFKGKATFTADTSSSTAYMQLNSLTSEDSGVYYCLHGNYDFDGWGQGTTLTVSS
```

**VL — cadeia leve variável**

```fasta
>HyHEL5_VL
MDIVLTQSPAIMSASPGEKVTMTCSASSSVNYMYWYQQKSGTSPKRWIYDTSKLASGVPVRFSGSGSGTSYSLTISSMETEDAATYYCQQWGRNPTFGGGTKLEIK
```

Para uso em um experimento diferente, substitua essas sequências pelas sequências VH e VL reais obtidas do seu conjunto de anticorpos, removendo as regiões constantes. Confirme que a sequência está no formato aceito pela ferramenta e que as cadeias pesada e leve foram inseridas nos campos corretos.

### 2. Sequência do antígeno para o BepiPred-3.0

```fasta
>HEL_lysozyme_P00698
KVFGRCELAAAMKRHGLDNYRGYSLGNWVCAAKFESNFNTQATNRNTDGSTDYGILQINSRWWCNDGRTPGSRNLCNIPCSALLSSDITASVNCAKKIVSDGNGMNAWVAWRNRCKGTDVQAWIRGCRL
```

Essa sequência tem 129 resíduos e corresponde à lisozima usada no complexo de referência. Ao comparar posições de resíduos, confirme sempre se a numeração da sequência coincide com a do PDB usado na etapa seguinte.

## Parte A — Modelagem do anticorpo com ImmuneBuilder

**Ferramenta:** [ImmuneBuilder](https://app.tamarind.bio/tools/immunebuilder)

1. Abra a ferramenta e selecione o modo para anticorpos (ABodyBuilder2, se a escolha for apresentada).
2. Cole a sequência **VH** no campo de cadeia pesada (*heavy chain*) e a sequência **VL** no campo de cadeia leve (*light chain*).
3. Execute a predição e salve o PDB gerado. Se a página disponibilizar pontuações de confiança por resíduo ou estimativas de variabilidade entre modelos, registre especialmente os resultados nas CDRs.
4. Anote se as seis CDRs aparecem no modelo e se alguma região apresenta confiança relativamente menor.

**Perguntas para discussão**

- Por que a sequência completa da cadeia de anticorpo, incluindo regiões constantes, não é o input adequado para uma ferramenta que modela apenas o Fv?
- Quais regiões do anticorpo são esperadas na interface com o antígeno?
- Uma estrutura globalmente plausível garante que a conformação das CDRs esteja correta?

**Produto desta etapa:** `HyHEL5_Fv_model.pdb` (nome sugerido; o nome real do arquivo pode ser diferente).

## Parte B — Predição de epítopos com BepiPred-3.0

**Ferramenta:** [BepiPred-3.0](https://services.healthtech.dtu.dk/services/BepiPred-3.0/)

1. Cole ou carregue a sequência FASTA da lisozima.
2. Execute a predição. Para a primeira análise, use o resultado por resíduo sem suavização sequencial, mais apropriado para observar candidatos a regiões conformacionais; depois, se houver tempo, compare com a saída suavizada, que enfatiza regiões lineares.
3. Visualize o gráfico e exporte o CSV de escores por resíduo. Selecione alguns trechos ou resíduos com escores elevados e registre suas posições.
4. Não escolha um epítopo apenas pelo maior pico: inspecione também se os resíduos correspondentes podem formar uma região exposta e espacialmente coerente na estrutura do antígeno.
5. Ao final da análise inicial, abra o complexo 1YQV no visualizador 3D e compare os resíduos previstos com a interface observada experimentalmente.

**Atenção:** o BepiPred prevê candidatos com base na sequência; ele não determina sozinho qual epítopo será reconhecido por um anticorpo específico. Os resíduos de um epítopo conformacional podem estar distantes na sequência e próximos no espaço. Escores elevados não são confirmação de ligação.

**Produto desta etapa:** gráfico/CSV de escores e uma lista curta de resíduos candidatos.

## Parte C — Docking com HDOCK

**Ferramenta:** [HDOCK](http://hdock.phys.hust.edu.cn/)

### Primeira rodada: docking sem restrições

1. Em **Input Receptor Molecule**, informe `1YQV:Y` como PDB ID:cadeia, ou use o arquivo PDB equivalente contendo apenas a cadeia do antígeno. No arquivo PDB original, a cadeia da lisozima é identificada por `Y`.
2. Em **Input Ligand Molecule**, carregue o PDB do Fv gerado pelo ImmuneBuilder. Mantenha as duas cadeias do Fv juntas no arquivo, se o modelo as fornecer como cadeias separadas.
3. Marque **Template-free docking only** para a rodada didática, reduzindo a possibilidade de o método recorrer a modelos estruturais de complexos conhecidos. Isso não elimina todas as fontes de viés, mas torna a comparação mais informativa.
4. Na primeira tentativa, deixe vazios os campos de resíduos do sítio de ligação e de restrições. O propósito é observar as poses geradas sem impor o epítopo antecipadamente.
5. Informe um nome para o trabalho e um e-mail, caso solicitado, e envie a tarefa.
6. Quando o trabalho terminar, baixe o complexo PDB da pose mais bem ranqueada. Se o servidor disponibilizar várias poses, baixe também algumas alternativas para comparar.

### Segunda rodada opcional: docking guiado

Se houver tempo, faça uma segunda rodada usando uma região exposta do antígeno apoiada pela análise do BepiPred e pela inspeção estrutural. Use a ferramenta de seleção de resíduos do HDOCK e siga exatamente o formato pedido pela interface. Compare o resultado guiado com a rodada sem restrições.

Não imponha um epítopo apenas para obter a pose desejada: a segunda rodada é um exercício para entender como restrições prévias influenciam o resultado.

### Como avaliar as poses

Baixe a estrutura da melhor pose e abra-a no [RCSB Mol* Viewer](https://www.rcsb.org/3d-view), usando a opção para carregar arquivos locais. Inspecione:

- se o anticorpo se aproxima do antígeno pelo lado das CDRs;
- se a pose apresenta colisões ou encaixe superficial pouco plausível;
- se a interface envolve vários resíduos próximos no espaço;
- se as poses bem ranqueadas são semelhantes entre si;
- se a localização da interface se aproxima da interface experimental de 1YQV.

Para comparação com o complexo experimental, abra também 1YQV no visualizador. Se o alinhamento automático não ocorrer, trate a comparação como qualitativa; não interprete uma sobreposição visual não alinhada como uma medida de RMSD.

**Produto desta etapa:** complexo(s) PDB, ranking de poses e uma justificativa breve para a pose selecionada.

## Parte D — Estimativa de energia de ligação por MM/GB(PB)SA

**Ferramenta:** [MM/GB(PB)SA — Tamarind Bio](https://app.tamarind.bio/tools/gbsa)

1. Selecione a tarefa **Protein-Protein**.
2. Carregue o PDB do complexo escolhido na etapa de docking.
3. Identifique os IDs de cadeia diretamente no arquivo PDB de saída. Insira a cadeia do antígeno em **Protein Chains** e a(s) cadeia(s) do anticorpo em **Binder Chains**. Não presuma que os IDs sejam os mesmos usados nos arquivos de entrada.
4. Para uma demonstração rápida, selecione o método de solvatação `gb` e o modo `em` (*energy minimization*), caso essas opções continuem disponíveis. O modo de minimização é adequado para uma atividade curta, mas é uma análise simplificada. Para uma investigação mais robusta, considere uma simulação de dinâmica molecular se houver tempo e recursos computacionais disponíveis.
5. Execute a tarefa, salve o resultado e registre o valor estimado de energia de ligação e as configurações usadas.
6. Se comparar poses, use o mesmo protocolo, método de solvatação e configuração para todas elas.

**Interpretação:** MM/GB(PB)SA fornece uma estimativa computacional dependente do modelo estrutural, da preparação e do protocolo. Um valor mais favorável não prova afinidade experimental maior e não deve ser interpretado como uma medição exata da energia livre de ligação. Não compare valores obtidos com configurações diferentes como se estivessem na mesma escala.

**Produto desta etapa:** tabela com nome da pose, configuração e resultado estimado.

## Parte E — Desenvolvibilidade com TAP e humanização com BioPhi

### TAP2 — Therapeutic Antibody Profiler

**Ferramenta:** [TAP2](https://app.tamarind.bio/tools/tap)

1. Insira VH e VL nos campos de cadeia pesada e leve, respectivamente; use as sequências variáveis, não as cadeias Fab completas.
2. Execute a análise e registre os alertas ou características destacadas pelo relatório.
3. Discuta quais características poderiam justificar experimentos posteriores de solubilidade, agregação, estabilidade ou ligação inespecífica, de acordo com o que a ferramenta efetivamente reportar.

TAP é um perfil preditivo de características associadas a anticorpos terapêuticos; não equivale a uma bateria experimental completa de desenvolvibilidade e não fornece, isoladamente, uma decisão de aprovação/reprovação do candidato.

### BioPhi — análise de humanização

**Ferramenta:** [BioPhi](https://app.tamarind.bio/tools/biophi)

1. Use VH e VL nos campos correspondentes.
2. Execute a avaliação inicial e anote as métricas disponibilizadas, incluindo OASis, se presentes.
3. Explore as recomendações de humanização, se a interface as disponibilizar. Compare as sequências antes e depois das alterações sugeridas.
4. Discuta quais mudanças poderiam afetar CDRs, resíduos do arcabouço ou a conformação do sítio de ligação. Humanização deve ser avaliada em conjunto com conservação da ligação e outras propriedades.

Como o HyHEL-5 é murino, esta etapa serve como demonstração de humanização. Uma sequência com pontuação de humanização mais favorável não tem, por isso só, imunogenicidade clínica baixa nem mantém necessariamente a afinidade.

## Entregáveis do estudo de caso 1

Cada grupo deve entregar:

1. modelo PDB do Fv e uma imagem anotada da estrutura;
2. gráfico/CSV do BepiPred com os resíduos candidatos assinalados;
3. imagem da pose de docking escolhida e comparação qualitativa com 1YQV;
4. tabela com os resultados de MM/GB(PB)SA, incluindo as configurações;
5. síntese dos relatórios TAP/BioPhi e uma conclusão de 5–8 linhas sobre o que os dados sugerem — e o que ainda não permitem concluir.

---

# Estudo de caso 2 — Design *de novo* de anticorpos

## Enunciado para os alunos

> Você identificou um alvo por triagem experimental, mas não encontrou moduladores descritos na literatura. Como poderia gerar candidatos a ligantes proteicos para testar contra esse alvo?

Para a aula, usaremos novamente a lisozima como um alvo-modelo. Nesta etapa, a entrada será a estrutura isolada **7S2Q, cadeia A**, e não o complexo 1YQV. A escolha é didática: permite desenhar contra uma superfície do antígeno sem fornecer à ferramenta a pose do anticorpo de referência.

## Parte A — Seleção da região-alvo

1. Abra [BepiPred-3.0](https://services.healthtech.dtu.dk/services/BepiPred-3.0/) e analise a mesma sequência da lisozima.
2. Abra [7S2Q](https://www.rcsb.org/structure/7S2Q) no [Mol* Viewer](https://www.rcsb.org/3d-view) e inspecione a superfície do antígeno.
3. Escolha uma região acessível e espacialmente coerente para testar. Use os escores do BepiPred como uma fonte de evidência, não como a única regra de escolha.
4. Se o exercício estiver sendo aplicado como uma simulação de alvo sem ligantes conhecidos, não mostre 1YQV antes dessa escolha. A comparação com o complexo conhecido pode ser feita depois.

**Como escolher hotspots:** selecione alguns resíduos de superfície que representem a região que o binder deverá reconhecer. Use o seletor de resíduos da ferramenta para evitar erros de numeração. Os hotspots devem pertencer à cadeia e à numeração do PDB efetivamente carregado. Não copie números de uma figura ou de outra estrutura sem conferir a correspondência.

## Parte B — Geração de candidatos com RFantibody

**Ferramenta:** [RFantibody — Tamarind Bio](https://app.tamarind.bio/tools/rfantibody)

1. Em **Framework**, mantenha `hu-4D5-8_Fv`, salvo orientação diferente do professor.
2. Em **Target File**, carregue o arquivo PDB de 7S2Q ou informe o PDB ID e faça o carregamento, se essa opção estiver disponível.
3. Em **Antigen Chain IDs**, selecione a cadeia `A` da lisozima.
4. Em **Hotspot Residues**, selecione os resíduos escolhidos na parte anterior. Use o seletor visual quando possível e confirme a numeração no PDB.
5. Em **Regions to design**, a configuração apresentada pode incluir `hcdr1,hcdr2,hcdr3,lcdr1,lcdr2,lcdr3`. Para a primeira rodada, use a configuração padrão sugerida pela ferramenta, a menos que o objetivo da aula seja estudar o efeito de redesenhar apenas algumas CDRs.
6. Mantenha os comprimentos de CDR em `auto` na primeira rodada, se disponível, para reduzir o número de parâmetros que os alunos precisam escolher.
7. Defina **Number of Designs** como 5–10 para uma aula. Isso serve para familiarização e comparação; não representa uma campanha de design de produção.
8. Envie a tarefa e guarde os arquivos de sequência e estrutura retornados para cada candidato.

> Se a ferramenta retornar poucos candidatos ou falhar, reduza o número de variáveis, confira a cadeia e os hotspots e tente novamente. Os tempos de execução e os limites podem variar conforme a disponibilidade da plataforma.

## Parte C — Inspeção e priorização dos candidatos

Para cada candidato gerado:

1. Abra o complexo PDB no Mol* Viewer.
2. Confirme se as CDRs redesenhadas estão orientadas para a região de hotspots definida.
3. Procure colisões evidentes, loops pouco plausíveis e interfaces muito pequenas ou dependentes de um único contato.
4. Compare candidatos **somente quando as condições de geração forem equivalentes** (mesmo alvo, hotspots, framework e configurações relevantes).
5. Escolha 1–3 candidatos para a análise complementar, justificando a escolha com base na estrutura e nas métricas efetivamente retornadas.

Não selecione o candidato apenas porque sua imagem parece mais convincente. Uma pose de aparência plausível pode não corresponder a uma interação física estável ou específica.

## Parte D — Perfil terapêutico e humanização

### TAP2

Use VH e VL de cada candidato priorizado no [TAP2](https://app.tamarind.bio/tools/tap), caso a sequência gerada esteja disponível nos resultados. Compare os relatórios e procure possíveis alertas associados ao perfil terapêutico.

### BioPhi

Use as sequências VH e VL no [BioPhi](https://app.tamarind.bio/tools/biophi). Registre a avaliação de humanização/OASis e examine recomendações, se oferecidas. Como o framework padrão `hu-4D5-8_Fv` é humanizado, esta análise pode ser mais útil como uma avaliação complementar do perfil da sequência do que como uma etapa de humanização completa. Alterações sugeridas precisam ser reavaliadas estruturalmente, sobretudo se afetarem regiões importantes para a ligação.

### MM/GB(PB)SA — atividade opcional

Se houver tempo, envie um dos complexos candidatos ao [MM/GB(PB)SA](https://app.tamarind.bio/tools/gbsa). Confirme os IDs das cadeias no arquivo de saída e use exatamente o mesmo protocolo utilizado para os outros candidatos. A estimativa é um critério auxiliar, não uma validação de afinidade.

## Entregáveis do estudo de caso 2

Cada grupo deve entregar:

1. imagem do antígeno com hotspots destacados e uma frase justificando a escolha;
2. tabela dos 5–10 designs gerados, incluindo identificador, observações estruturais e critérios usados para priorização;
3. imagem anotada dos 1–3 candidatos selecionados;
4. síntese dos resultados de TAP/BioPhi e, se realizado, MM/GB(PB)SA;
5. uma breve proposta de validação experimental para testar expressão, pureza, ligação e especificidade dos candidatos.

---

# Planilha simples de registro de resultados

Preencha uma linha por modelo ou candidato. Acrescente as métricas realmente retornadas pela ferramenta e deixe em branco os campos não disponibilizados.

| Campo | Registro |
|---|---|
| Estudo de caso | 1 / 2 |
| Identificador do modelo/candidato |  |
| Ferramenta e versão/data de execução |  |
| Sequência ou estrutura de entrada |  |
| Alvo/cadeia PDB |  |
| Hotspots/restrições utilizados |  |
| Métrica de confiança estrutural |  |
| Score/ranking de docking |  |
| Observação sobre orientação das CDRs |  |
| Colisões ou problemas visíveis |  |
| MM/GB(PB)SA e protocolo |  |
| Resultado TAP |  |
| Resultado BioPhi/OASis |  |
| Decisão de priorização e justificativa |  |
| Principal limitação da análise |  |

---

# Perguntas para discussão final

1. O que muda entre a previsão de um epítopo e a identificação experimental de um epítopo reconhecido por um anticorpo específico?
2. Uma pose de docking bem ranqueada necessariamente reproduz a estrutura experimental? Por quê?
3. Qual é a diferença entre um score de docking, uma estimativa de MM/GB(PB)SA e uma afinidade experimental como Kd?
4. Em que pontos a incerteza estrutural das CDRs pode afetar a seleção de candidatos?
5. Quais características de TAP e BioPhi são úteis para priorizar sequências e quais exigem confirmação adicional?
6. Quais testes experimentais seriam necessários antes de chamar um candidato de binder específico ou potencial terapêutico?

## Mensagem central

O fluxo computacional serve para **formular hipóteses, visualizar interfaces e priorizar candidatos**. Predição de epítopos, modelagem estrutural, docking, MM/GB(PB)SA, TAP e BioPhi respondem a perguntas diferentes e têm limitações próprias. Nenhuma dessas etapas, isoladamente ou em conjunto, demonstra que um anticorpo se liga ao alvo, neutraliza uma função biológica, é seguro ou terá desempenho terapêutico. Essas afirmações exigem validação experimental adequada.

---

## Fontes e páginas das ferramentas

- [RCSB PDB — 1YQV: complexo HyHEL-5–lisozima](https://www.rcsb.org/structure/1YQV)
- [RCSB PDB — 7S2Q: lisozima isolada](https://www.rcsb.org/structure/7S2Q)
- [ImmuneBuilder online — Tamarind Bio](https://app.tamarind.bio/tools/immunebuilder)
- [BepiPred-3.0 — servidor web oficial](https://services.healthtech.dtu.dk/services/BepiPred-3.0/)
- [BepiPred-3.0 — código-fonte no GitHub](https://github.com/UberClifford/BepiPred-3.0)
- [HDOCK](http://hdock.phys.hust.edu.cn/)
- [RCSB Mol* Viewer](https://www.rcsb.org/3d-view)
- [MM/GB(PB)SA — Tamarind Bio](https://app.tamarind.bio/tools/gbsa)
- [TAP2 — Tamarind Bio](https://app.tamarind.bio/tools/tap)
- [BioPhi — Tamarind Bio](https://app.tamarind.bio/tools/biophi)
- [RFantibody — Tamarind Bio](https://app.tamarind.bio/tools/rfantibody)
