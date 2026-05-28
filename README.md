# Minicurso: Limitações do AlphaFold na Predição de Estruturas Proteicas

> Material prático desenvolvido para acompanhar a aula teórica sobre AlphaFold.  
> Nenhum conhecimento prévio de bioinformática é necessário — apenas acesso a um navegador.

---

## Sobre o minicurso

O AlphaFold revolucionou a biologia estrutural. Mas como qualquer modelo de aprendizado de máquina, ele tem pontos cegos — e entender esses limites é tão importante quanto saber usar a ferramenta.

Nesta prática, você vai modelar proteínas reais **sem saber o contexto por trás delas**. Depois, vamos comparar as predições com estruturas cristalográficas experimentais e discutir o que o AlphaFold acertou, errou, e por quê.

**Três casos, três tipos de limitação:**

| Caso | Sistema | Limitação explorada |
|------|---------|-------------------|
| 1 | p53 (WT vs Y220C) | Mutação pontual que cria cavidade estrutural |
| 2 | Complexo anticorpo–antígeno (anti-PD-1) | Pose de interação incorreta com confiança alta |
| 3 | RfaH (fator de transcrição bacteriano) | Proteína com dois folds reais — AF escolhe um |

---

## Ferramenta utilizada

**AlphaFold 3 Server** — acesso gratuito pelo navegador, sem instalação.

🔗 [alphafold3.google.com](https://alphafold3.google.com)

> Você precisará de uma conta Google para acessar. As predições levam entre 2–5 minutos cada.

---

## Como usar este repositório

Cada caso está em sua própria pasta com:
- `sequencias.txt` — sequências para submeter no AF3
- `instrucoes.md` — o que observar e anotar
- `pdbs/` — estruturas experimentais de referência (WT e mutante)
- `discussao.md` — perguntas e contexto revelado após a prática *(abrir só depois!)*

```
alphafold-minicurso/
├── README.md
├── caso1_p53/
│   ├── sequencias.txt
│   ├── instrucoes.md
│   ├── discussao.md
│   └── pdbs/
│       ├── 2OCJ_WT.pdb
│       └── 2VUK_Y220C.pdb
├── caso2_anticorpo/
│   ├── sequencias.txt
│   ├── instrucoes.md
│   ├── discussao.md
│   └── pdbs/
│       └── 6J15_crystal.pdb
└── caso3_RfaH/
    ├── sequencias.txt
    ├── instrucoes.md
    ├── discussao.md
    └── pdbs/
        ├── 2OUG_alpha.pdb
        └── 6C6S_beta.pdb
```

---

## Caso 1 — p53: uma mutação, uma cavidade, 75 mil novos casos por ano

### Contexto (revelar só na discussão)

O p53 é o supressor tumoral mais mutado em câncer humano. A mutação **Y220C** substitui uma tirosina volumosa por uma cisteína menor, criando uma cavidade de ~200 Å³ na superfície da proteína. Essa cavidade desestabiliza o dobramento em 4 kcal/mol e inativa o p53 em dezenas de tipos de tumor.

O AF3 prediz o mutante com estrutura praticamente idêntica ao WT — a cavidade é invisível para o modelo.

### Sequências para submeter

Abrir [`caso1_p53/sequencias.txt`](caso1_p53/sequencias.txt)

> Submeta a **Proteína A** e depois a **Proteína B** separadamente. Não pesquise os nomes ainda.

### Estruturas experimentais

| Estrutura | PDB | Descrição |
|-----------|-----|-----------|
| p53 DBD WT | [2OCJ](https://www.rcsb.org/structure/2OCJ) | Domínio de ligação ao DNA selvagem |
| p53 Y220C | [2VUK](https://www.rcsb.org/structure/2VUK) | Mutante com cavidade visível |
| p53 Y220C | [6SHZ](https://www.rcsb.org/structure/6SHZ) | Forma alternativa, alta resolução |

---

## Caso 2 — Anticorpo anti-PD-1: confiança alta, pose errada

### Contexto (revelar só na discussão)

O PD-1 é um checkpoint imunológico central na terapia de câncer. Anticorpos que bloqueiam PD-1 estão entre os medicamentos oncológicos mais usados no mundo (nivolumabe, pembrolizumabe).

O AF3 prediz o anticorpo e o antígeno com estruturas individuais corretas — mas quando modelados em complexo, a **pose de encaixe está errada**. O anticorpo toca a região certa do antígeno, mas com rotação e posicionamento diferentes do cristal experimental (I-RMSD ~3.5 Å na interface).

Se você usasse essa predição para desenhar um anticorpo melhorado, identificaria os resíduos de contato errados.

### Sequências para submeter

Abrir [`caso2_anticorpo/sequencias.txt`](caso2_anticorpo/sequencias.txt)

> Submeta as três cadeias juntas (cadeia H + cadeia L + antígeno) como complexo proteico.

### Estruturas experimentais

| Estrutura | PDB | Descrição |
|-----------|-----|-----------|
| Anti-PD-1 + PD-1 | [6J15](https://www.rcsb.org/structure/6J15) | Cristal real do complexo |
| Exemplo boa predição AF | [6NMV](https://www.rcsb.org/structure/6NMV) | Referência: caso onde AF acerta |

---

## Caso 3 — RfaH: quando uma sequência tem dois folds reais

### Contexto (revelar só na discussão)

RfaH é um fator de transcrição/tradução bacteriano cujo domínio C-terminal (CTD) existe em **dois folds completamente diferentes**:

- **Forma α (autoinibida):** duas hélices, proteína sozinha em solução
- **Forma β (ativa):** barrel de fitas β, quando ligada à RNA polimerase

Mais de 60% dos resíduos mudam de estrutura secundária entre as duas formas. O AF3 prediz quase exclusivamente a forma β com pLDDT alto — porque esse fold está mais representado no conjunto de treinamento. A forma α, que é a estrutura dominante em solução, é ignorada.

### Sequência para submeter

Abrir [`caso3_RfaH/sequencias.txt`](caso3_RfaH/sequencias.txt)

> Submeta apenas o domínio C-terminal (CTD). Anote a estrutura secundária predita: hélices ou fitas?

### Estruturas experimentais

| Estrutura | PDB | Descrição |
|-----------|-----|-----------|
| RfaH CTD α-helicoidal | [2OUG](https://www.rcsb.org/structure/2OUG) | Forma autoinibida (solução) |
| RfaH CTD β-roll | [6C6S](https://www.rcsb.org/structure/6C6S) | Forma ativa (ligada à RNAP) |

---

## Visualização das estruturas

Para visualizar e comparar os PDBs no navegador, recomendamos:

- **[Mol* Viewer](https://molstar.org/viewer/)** — arraste o arquivo .pdb direto para a janela
- **[RCSB PDB 3D viewer](https://www.rcsb.org)** — clique em qualquer código PDB acima
- **[iCn3D](https://www.ncbi.nlm.nih.gov/Structure/icn3d/)** — opção do NCBI, boa para comparações

Para sobrepor duas estruturas e calcular RMSD:
```
# No Mol*, carregue dois arquivos e use:
# Extensions > Superposition
```

---

## Roteiro geral da prática

Para cada caso, siga esta sequência:

1. **Submeta** as sequências no AF3 sem ler o contexto
2. **Anote** para cada predição:
   - pLDDT médio e regiões de baixa confiança
   - Estrutura secundária predominante (hélices, fitas, loops)
   - Diferenças visuais entre as sequências comparadas
3. **Responda** às perguntas do arquivo `instrucoes.md`
4. **Só então** abra o `discussao.md` e compare com o experimental

---

## Perguntas unificadoras

Ao final dos três casos, reflita:

> O pLDDT foi alto nos três casos — mesmo onde o AF errou. O que isso nos diz sobre o que o pLDDT mede, e o que ele **não** mede?

> O AlphaFold "errou" nesses casos, ou simplesmente respondeu uma pergunta diferente da que queríamos fazer?

> Para que tipos de problemas você **confiaria** plenamente em uma predição do AF? Para quais exigiria validação experimental?

---

## Referências

Os casos desta prática são baseados nos seguintes trabalhos:

- **Caso 1:** Buel & Walters (2022). *Can AlphaFold2 predict the impact of missense mutations on structure?* Nature Structural & Molecular Biology. [PMC11218004](https://pmc.ncbi.nlm.nih.gov/articles/PMC11218004/)
- **Caso 1:** Joerger et al. (2024). *Structural basis of p53 inactivation by cavity-creating cancer mutations.* Cell Death & Disease. [10.1038/s41419-024-06739-x](https://doi.org/10.1038/s41419-024-06739-x)
- **Caso 2:** Yin & Pierce (2024). *Evaluation of AlphaFold antibody–antigen modeling.* Protein Science. [10.1002/pro.4865](https://doi.org/10.1002/pro.4865)
- **Caso 3:** Chakravarty & Porter (2022). *AlphaFold2 fails to predict protein fold switching.* Protein Science. [10.1002/pro.4353](https://doi.org/10.1002/pro.4353)
- **Geral:** Pak et al. (2023). *Using AlphaFold to predict the impact of single mutations on protein stability and function.* PLOS ONE. [PMC10019719](https://www.ncbi.nlm.nih.gov/pmc/articles/PMC10019719/)

---

## Créditos

Material desenvolvido para o minicurso **"Predição de estruturas proteicas: potencial e limitações do AlphaFold"**.  
Dúvidas? Abra uma [issue](../../issues) neste repositório.

---

<sub>As estruturas PDB utilizadas neste material estão disponíveis publicamente no [RCSB Protein Data Bank](https://www.rcsb.org) sob licença CC0. As sequências são derivadas das entradas UniProt correspondentes.</sub>
