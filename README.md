## Bioinformática estrutural aplicada a anticorpos: __da sequência do anticorpo à análise da interface e ao design *de novo*__

*Sistema-modelo escolhido*

A lisozima da clara do ovo de galinha (HEL) é uma enzima antimicrobiana que atua na imunidade inata, catalisando a quebra de ligações β(1→4) no peptidoglicano, um componente importante da parede celular bacteriana. Essa atividade pode comprometer a integridade da parede e contribuir para a lise bacteriana.

Com 129 aminoácidos e aproximadamente 14,3 kDa, a HEL é uma proteína pequena e bem caracterizada, com diversas estruturas tridimensionais experimentais disponíveis no Protein Data Bank (PDB). Sua estrutura cristalográfica isolada pode ser acessada pelo código 7S2Q, determinada por difração de raios X com resolução de 1,60 Å.

Além de sua relevância biológica, a HEL é um modelo amplamente utilizado em estudos de reconhecimento molecular e interação anticorpo–antígeno. Um exemplo é o complexo cristalográfico entre a lisozima e o fragmento Fab do anticorpo HyHEL-5, disponível no PDB sob o código 1YQV, com resolução de 1,70 Å. Essa estrutura permite examinar a interface de interação experimentalmente observada e compará-la com modelos computacionais.

### Ferramentas

| Etapa | Ferramenta | Finalidade |
|---|---|---|
| Modelagem de anticorpo | [ABodyBuilder2 — OPIG/SAbPred](https://opig.stats.ox.ac.uk/webapps/sabdab-sabpred/sabpred/abodybuilder2/) | Predizer a estrutura do Fv a partir das sequências VH e VL |
| Avaliação de qualidade estrutural | [QMEANDisCo](https://swissmodel.expasy.org/qmean) | Avaliar qualidade global e por resíduo do modelo estrutural |
| Predição de epítopos | [BepiPred-3.0 — servidor web da DTU](https://services.healthtech.dtu.dk/services/BepiPred-3.0/) | Estimar resíduos candidatos a epítopos de células B a partir da sequência do antígeno |
| Docking | [ClusPro2.0](https://cluspro.org/) | Gerar poses de interação entre antígeno e anticorpo |
| Visualização de estruturas | [RCSB Mol* 3D Viewer](https://www.rcsb.org/3d-view) | Visualizar PDBs locais e estruturas depositadas no PDB |
| Energia de ligação estimada | [MM/GB(PB)SA — Tamarind Bio](https://app.tamarind.bio/tools/gbsa) | Estimar energia de ligação a partir de um complexo proteico |
| Perfil de desenvolvibilidade | [TAP2 — Therapeutic Antibody Profiler](https://app.tamarind.bio/tools/tap) | Avaliar características relacionadas ao perfil de anticorpos terapêuticos |
| Humanização | [BioPhi — Tamarind Bio](https://app.tamarind.bio/tools/biophi) | Avaliar a sequência e explorar recomendações de humanização |
| Design *de novo* | [RFantibody — Tamarind Bio](https://app.tamarind.bio/tools/rfantibody) | Gerar modelos e sequências candidatas de anticorpos contra um antígeno |

As atividades desta aula priorizam ferramentas online. Entretanto, caso deseje explorar as estruturas proteicas com mais detalhes, você pode baixar e instalar o UCSF ChimeraX, um programa de visualização molecular que permite inspecionar interativamente estruturas PDB, domínios de anticorpos, CDRs, epítopos e interfaces anticorpo–antígeno. Seu uso é opcional, mas pode facilitar a análise de detalhes estruturais que nem sempre são facilmente visualizados diretamente no navegador. Faça o download gratuito [aqui](https://www.cgl.ucsf.edu/chimerax/download.html).

### Estudo de caso 1 — Mapeamento de interações anticorpo–antígeno

> Você selecionou anticorpos por *phage display* e biopanning contra um alvo de interesse e sequenciou as regiões variáveis. Agora precisa investigar como um anticorpo se estrutura, onde pode se ligar ao antígeno e quais aspectos merecem avaliação antes de considerar uma aplicação terapêutica.

Neste exercício, as sequências variáveis do HyHEL-5 serão usadas como exemplo positivo. O objetivo é praticar o fluxo de análise e comparar as previsões com uma estrutura experimental conhecida.

**Arquivos e sequências de entrada**

*1. Sequências variáveis de anticorpo*

VH — cadeia pesada variável

```fasta
>HyHEL5_VH
EVQLQQSGAELMKPGASVKISCKASGYTFSDYWIEWVKQRPGHGLEWIGEILPGSGSTNYHERFKGKATFTADTSSSTAYMQLNSLTSEDSGVYYCLHGNYDFDGWGQGTTLTVSS
```

VL — cadeia leve variável

```fasta
>HyHEL5_VL
MDIVLTQSPAIMSASPGEKVTMTCSASSSVNYMYWYQQKSGTSPKRWIYDTSKLASGVPVRFSGSGSGTSYSLTISSMETEDAATYYCQQWGRNPTFGGGTKLEIK
```

Para uso em um experimento diferente, substitua essas sequências pelas sequências VH e VL reais obtidas do seu conjunto de anticorpos, removendo as regiões constantes.

*2. Sequência do antígeno para o BepiPred-3.0*

```fasta
>HEL_lysozyme_P00698
KVFGRCELAAAMKRHGLDNYRGYSLGNWVCAAKFESNFNTQATNRNTDGSTDYGILQINSRWWCNDGRTPGSRNLCNIPCSALLSSDITASVNCAKKIVSDGNGMNAWVAWRNRCKGTDVQAWIRGCRL
```

**A1. Gerar o modelo do Fv**

1. Abra o servidor ABodyBuilder2 da OPIG.
2. Cole a sequência **VH** no campo *Heavy chain sequence* e a sequência **VL** no campo *Light chain sequence*. Use as sequências variáveis fornecidas acima, sem as regiões constantes.
3. Escolha uma convenção de numeração (por exemplo, IMGT) e registre qual foi utilizada. A ferramenta pode renumerar as estruturas com ANARCI.
4. Execute **Model** e baixe o modelo PDB gerado. Registre as métricas de confiança/erro disponibilizadas pelo servidor, se presentes, e observe especialmente as CDRs.

**Produto desta etapa:** arquivo PDB de estrutura do Fv do anticorpo.

**A2. Avaliar a qualidade do modelo com QMEANDisCo**

1. Acesse o SWISS-MODEL Structure Assessment e envie o PDB gerado pelo ABodyBuilder2.
2. Execute a avaliação e registre o **QMEANDisCo global** e os escores locais por resíduo, se apresentados. Examine também os indicadores adicionais de geometria/estereoquímica disponíveis na página.
3. Identifique regiões com qualidade local relativamente baixa, prestando atenção especial às CDRs, sobretudo à CDR-H3, e compare a avaliação do QMEANDisCo com as métricas de confiança fornecidas pelo ABodyBuilder2, caso existam. Eles apontam para as mesmas regiões problemáticas?

**Perguntas para discussão**

- Por que a sequência completa do anticorpo, incluindo regiões constantes, não é o input adequado para uma ferramenta que modela apenas o Fv?
- Um modelo globalmente bem avaliado necessariamente possui CDRs bem modeladas?
- Como incertezas locais no Fv podem afetar a posição e a interpretação do complexo produzido no docking?

**Parte B — Predição de epítopos com BepiPred-3.0**

**Ferramenta:** [BepiPred-3.0](https://services.healthtech.dtu.dk/services/BepiPred-3.0/)

1. Insira a sequência FASTA da lisozima e execute a predição.
2. Examine o gráfico de escores por resíduo e identifique as posições que ultrapassam o limiar selecionado (0,25063 neste exemplo). Registre as regiões candidatas e seus respectivos resíduos.
3. Mapeie os resíduos previstos na estrutura tridimensional da lisozima e verifique quais estão expostos ao solvente e se formam uma região espacialmente coerente. Compare as regiões previstas com a interface experimental do complexo HyHEL-5–lisozima (PDB: 1YQV). 

**Atenção:** o BepiPred prevê candidatos com base na sequência; ele não determina sozinho qual epítopo será reconhecido por um anticorpo específico. Os resíduos de um epítopo conformacional podem estar distantes na sequência e próximos no espaço. Escores elevados não são confirmação de ligação.

**Produto desta etapa:** gráfico/CSV de escores e uma lista curta de resíduos candidatos.

**Parte C — Docking proteína–proteína com ClusPro (Antibody Mode)**

**Ferramenta:** [ClusPro](https://cluspro.org/)

Nesta etapa, será realizado o docking entre o Fv modelado e a lisozima, utilizando o modo específico para anticorpos do ClusPro. Diferentemente do docking dirigido por resíduos, não será imposto previamente o epítopo identificado pelo BepiPred-3.0. A localização da interface será investigada posteriormente, por meio da inspeção das poses geradas.

*Procedimento*

1. Acesse o ClusPro e faça login ou crie uma conta, caso necessário. Verifique as condições de acesso e a disponibilidade do serviço.
2. Em **Job Name**, informe um nome para a tarefa, por exemplo, `Lysozyme_Antibody_Docking`.
3. Em **Receptor**, carregue o arquivo PDB do anticorpo modelado pelo ImmuneBuilder, contendo as cadeias pesada e leve do Fv.
4. Em **Ligand**, informe o código PDB `1YQV` e selecione a cadeia `Y`, correspondente à lisozima, ou carregue um arquivo PDB equivalente contendo apenas essa cadeia.
5. Em **Advanced Options → Antibody Mode**, marque **Use Antibody Mode**. O ClusPro recomenda que o anticorpo seja definido como receptor e o antígeno como ligante. Mantenha essa orientação e, se possível, utilize também a opção **Automatically Mask non-CDR regions**, que reduz a participação das regiões não pertencentes às CDRs na busca por interfaces.
6. Não informe restrições de resíduos baseadas no BepiPred-3.0 nesta etapa. O objetivo é permitir que o programa explore possíveis orientações de interação sem impor previamente a região do epítopo.
7. Submeta o trabalho e aguarde o processamento. Quando os resultados estiverem disponíveis, baixe os modelos dos clusters mais bem ranqueados e selecione poses representativas para análise estrutural.

*Análise das poses e comparação com o BepiPred-3.0*

1. Abra as poses selecionadas em um visualizador molecular, como o [UCSF ChimeraX](https://www.cgl.ucsf.edu/chimerax/) ou o [RCSB Mol* Viewer](https://www.rcsb.org/3d-view).
2. Verifique se as poses mais bem ranqueadas apresentam orientações semelhantes e se o anticorpo se aproxima da lisozima pelas regiões das CDRs.
3. Identifique os resíduos da lisozima que participam da interface em cada pose e compare-os com as regiões candidatas identificadas pelo BepiPred-3.0.

**Produto desta etapa:** complexo(s) PDB e ranking de poses.

**Parte D — Estimativa de energia de ligação por MM/GB(PB)SA**

**Ferramenta:** [MM/GB(PB)SA — Tamarind Bio](https://app.tamarind.bio/tools/gbsa)

1. Selecione a tarefa **Protein-Protein**.
2. Carregue o PDB do complexo escolhido na etapa de docking.
3. Identifique os IDs de cadeia diretamente no arquivo PDB de saída. Insira a cadeia do antígeno em **Protein Chains** e a(s) cadeia(s) do anticorpo em **Binder Chains**. Não presuma que os IDs sejam os mesmos usados nos arquivos de entrada.
4. Para uma demonstração rápida, selecione o método de solvatação `gb` e o modo `em` (*energy minimization*), caso essas opções continuem disponíveis. O modo de minimização é adequado para uma atividade curta, mas é uma análise simplificada. Para uma investigação mais robusta, considere uma simulação de dinâmica molecular se houver tempo e recursos computacionais disponíveis.
5. Execute a tarefa, salve o resultado e registre o valor estimado de energia de ligação e as configurações usadas.
6. Se comparar poses, use o mesmo protocolo, método de solvatação e configuração para todas elas.

**Interpretação:** MM/GB(PB)SA fornece uma estimativa computacional dependente do modelo estrutural, da preparação e do protocolo. Um valor mais favorável não prova afinidade experimental maior e não deve ser interpretado como uma medição exata da energia livre de ligação. Não compare valores obtidos com configurações diferentes como se estivessem na mesma escala.

**Produto desta etapa:** tabela com nome da pose, configuração e resultado estimado.

**Parte E — Desenvolvibilidade com TAP e humanização com BioPhi**

**Ferramenta:** [TAP2](https://app.tamarind.bio/tools/tap)

1. Insira VH e VL nos campos de cadeia pesada e leve, respectivamente; use as sequências variáveis, não as cadeias Fab completas.
2. Execute a análise e registre os alertas ou características destacadas pelo relatório.
3. Discuta quais características poderiam justificar experimentos posteriores de solubilidade, agregação, estabilidade ou ligação inespecífica, de acordo com o que a ferramenta efetivamente reportar.

TAP é um perfil preditivo de características associadas a anticorpos terapêuticos; não equivale a uma bateria experimental completa de desenvolvibilidade e não fornece, isoladamente, uma decisão de aprovação/reprovação do candidato.

**Ferramenta:** [BioPhi](https://app.tamarind.bio/tools/biophi)

1. Use VH e VL nos campos correspondentes.
2. Execute a avaliação inicial e anote as métricas disponibilizadas, incluindo OASis, se presentes.
3. Explore as recomendações de humanização, se a interface as disponibilizar. Compare as sequências antes e depois das alterações sugeridas.
4. Discuta quais mudanças poderiam afetar CDRs, resíduos do arcabouço ou a conformação do sítio de ligação. Humanização deve ser avaliada em conjunto com conservação da ligação e outras propriedades.

Como o HyHEL-5 é murino, esta etapa serve como demonstração de humanização. Uma sequência com pontuação de humanização mais favorável não tem, por isso só, imunogenicidade clínica baixa nem mantém necessariamente a afinidade.

---

### Estudo de caso 2 — Design *de novo* de anticorpos

> Você identificou um alvo por triagem experimental, mas não encontrou moduladores descritos na literatura. Como poderia gerar candidatos a ligantes proteicos para testar contra esse alvo?

Para a aula, usaremos novamente a lisozima como um alvo-modelo. Nesta etapa, a entrada será a estrutura isolada **7S2Q, cadeia A**, e não o complexo 1YQV. A escolha é didática: permite desenhar contra uma superfície do antígeno sem fornecer à ferramenta a pose do anticorpo de referência.

**Parte A — Seleção da região-alvo**

Abra [7S2Q](https://www.rcsb.org/structure/7S2Q) no [Mol* Viewer](https://www.rcsb.org/3d-view) e inspecione a superfície do antígeno e a sequência para verificar quais resíduos correspondem aqueles identificados pelo preditor de epítopos. Use o seletor de resíduos da ferramenta para evitar erros de numeração. Os hotspots devem pertencer à cadeia e à numeração do PDB efetivamente carregado. Não copie números de uma figura ou de outra estrutura sem conferir a correspondência.

**Parte B — Geração de candidatos com RFantibody**

**Ferramenta:** [RFantibody — Tamarind Bio](https://app.tamarind.bio/tools/rfantibody)

1. Em **Framework**, mantenha `hu-4D5-8_Fv`.
2. Em **Target File**, carregue o arquivo PDB de 7S2Q ou informe o PDB ID e faça o carregamento, se essa opção estiver disponível.
3. Em **Antigen Chain IDs**, selecione a cadeia `A` da lisozima.
4. Em **Hotspot Residues**, selecione os resíduos escolhidos na parte anterior. Use o seletor visual quando possível e confirme a numeração no PDB (45 46 47 48 49 68 73 102 107).
5. Em **Regions to design**, a configuração apresentada pode incluir `hcdr1,hcdr2,hcdr3,lcdr1,lcdr2,lcdr3`. Para a primeira rodada, use a configuração padrão sugerida pela ferramenta.
6. Mantenha os comprimentos de CDR em `auto` na primeira rodada.
7. Defina **Number of Designs** entre 3 e 10.
8. Envie a tarefa e guarde os arquivos de sequência e estrutura retornados para cada candidato.

> Se a ferramenta retornar poucos candidatos ou falhar, reduza o número de variáveis, confira a cadeia e os hotspots e tente novamente. Os tempos de execução e os limites podem variar conforme a disponibilidade da plataforma.

**Parte C — Inspeção e priorização dos candidatos**

Para cada candidato gerado:

1. Abra o complexo PDB no Mol* Viewer.
2. Confirme se as CDRs redesenhadas estão orientadas para a região de hotspots definida.
3. Procure colisões evidentes, loops pouco plausíveis e interfaces muito pequenas ou dependentes de um único contato.
4. Compare candidatos **somente quando as condições de geração forem equivalentes** (mesmo alvo, hotspots, framework e configurações relevantes).
5. Escolha 1–3 candidatos para a análise complementar, justificando a escolha com base na estrutura e nas métricas efetivamente retornadas.

Não selecione o candidato apenas porque sua imagem parece mais convincente. Uma pose de aparência plausível pode não corresponder a uma interação física estável ou específica.

**Parte D — Perfil terapêutico e humanização**

TAP2

Use VH e VL de cada candidato priorizado no [TAP2](https://app.tamarind.bio/tools/tap), caso a sequência gerada esteja disponível nos resultados. Compare os relatórios e procure possíveis alertas associados ao perfil terapêutico.

BioPhi

Use as sequências VH e VL no [BioPhi](https://app.tamarind.bio/tools/biophi). Registre a avaliação de humanização/OASis e examine recomendações, se oferecidas. Como o framework padrão `hu-4D5-8_Fv` é humanizado, esta análise pode ser mais útil como uma avaliação complementar do perfil da sequência do que como uma etapa de humanização completa. Alterações sugeridas precisam ser reavaliadas estruturalmente, sobretudo se afetarem regiões importantes para a ligação.

