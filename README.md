# Reator Entrópico

Jogo educativo cooperativo de termodinâmica para duas pessoas. Uma pessoa opera o painel de um reator em falha enquanto a outra consulta um manual com as informações que faltam. A dupla precisa se comunicar para restaurar sete módulos antes que o tempo ou a margem de erros acabem.

O projeto inteiro está contido em [`index.html`](./index.html): menu, jogo do Operador, manual do Especialista, estilos, regras e efeitos sonoros. Não há backend, processo de build ou dependências para instalar.

## Como executar

1. Abra `index.html` em um navegador moderno.
2. Em um dispositivo ou aba, escolha **Iniciar como Operador**.
3. Abra o arquivo novamente em outro dispositivo ou aba e escolha **Iniciar como Especialista (Manual)**.
4. Mantenham as telas separadas e conversem por voz. O Operador informa os dados visíveis; o Especialista consulta ou calcula a resposta no manual.

Também é possível servir a pasta localmente, embora isso não seja obrigatório:

```bash
python -m http.server 8000
```

Depois, acesse `http://localhost:8000/`.

> O navegador pode exigir uma primeira interação para liberar o áudio. As fontes JetBrains Mono e Orbitron são carregadas do Google Fonts; sem internet, a aplicação continua funcionando com as fontes alternativas do sistema.

## Objetivo e regras

- Restaurar os **7 módulos** e então acionar o reator.
- Tempo inicial: **8 minutos**.
- Cada resposta incorreta adiciona um marcador de erro e desconta **30 segundos**.
- A partida termina em colapso ao chegar a **3 erros** ou quando o cronômetro zera.
- Os módulos podem ser resolvidos em qualquer ordem.
- Uma nova partida gera outro número de série, modelo de reator, desafios e configurações aleatórias.
- O estado existe apenas em memória: recarregar a página reinicia a experiência.

O painel reage ao risco com cronômetro, LEDs, vinheta, tremores, glitches e uma trilha ambiente sintetizada. Ao restaurar todos os módulos, o botão final é liberado e a tela de vitória mostra o tempo e a quantidade de erros.

## Módulos

| Módulo | Tema | Desafio do Operador | Papel do Especialista |
| --- | --- | --- | --- |
| MOD-A | Calor | Resolver 3 cenários de direção do fluxo térmico | Revelar a temperatura oculta pelo ID e comparar os reservatórios |
| MOD-B | Temperatura | Calibrar 6 sensores sorteados | Converter entre °C, K e °F; o jogo aceita margem de ±1 |
| MOD-C | Primeira Lei | Resolver 3 balanços ajustando `ΔU` | Obter `Q` pela reação e calcular `ΔU = Q − W`; margem de ±3 J |
| MOD-D | Entropia | Ordenar 5 processos do maior para o menor `ΔS` | Informar a ordem correta dos IDs `PROC-01` a `PROC-05` |
| MOD-E | Ciclo termodinâmico | Classificar 4 processos sorteados | Consultar o modelo ALFA, BETA, GAMA ou DELTA e identificar cada processo |
| MOD-F | Código de ignição | Digitar uma sequência de 5 símbolos | Derivar o código a partir da soma e das vogais do número de série |
| MOD-G | Válvulas | Configurar 6 válvulas inicialmente aleatórias | Derivar os estados usando série, soma dos dígitos e modelo do reator |

O manual do Especialista inclui as tabelas de reservatórios e reações, fórmulas de conversão, classificação de entropia, mapas dos quatro modelos, regras do código de ignição, regras das válvulas e uma referência rápida para consulta durante a partida.

## Como o arquivo funciona

A página inicial guarda duas páginas HTML completas em strings JavaScript:

- `htmlOperador`: interface interativa, estado da partida, sorteios, validações, cronômetro, penalidades, animações e áudio;
- `htmlEspecialista`: manual de consulta responsivo e preparado para impressão.

Ao escolher um papel, `document.write()` substitui o menu pela página correspondente. No modo Operador, o estado global controla módulos concluídos, tempo restante, erros, serial e modelo. Cada módulo possui uma função de montagem e outra de validação; uma resposta correta atualiza o progresso, enquanto uma incorreta chama o sistema central de penalidade.

O áudio é produzido em tempo real com a Web Audio API, sem arquivos de mídia. Todo o restante usa somente HTML, CSS e JavaScript nativos. O único recurso externo são as fontes web.

## Compatibilidade e uso em aula

- Recomendado: versões atuais de Chrome, Edge ou Firefox em desktop.
- O MOD-D usa a API nativa de arrastar e soltar, portanto a experiência é mais confiável com mouse ou trackpad.
- Para jogar em dois dispositivos, disponibilize o mesmo arquivo em ambos ou use um servidor HTTP acessível na rede local.
- O botão **Fechar Sistema** pode não fechar uma aba aberta manualmente devido às regras de segurança do navegador; nesse caso, a própria página exibe uma mensagem de encerramento.
- O manual possui estilos de impressão e pode ser impresso ou salvo como PDF pelo navegador.

## Estrutura do repositório

```text
.
├── index.html  # aplicação completa
└── README.md   # documentação do projeto
```

## Conteúdos trabalhados

O jogo aborda transferência espontânea de calor, equilíbrio térmico, escalas de temperatura, Primeira Lei da Termodinâmica, variação de entropia e processos isotérmico, isobárico, isocórico e adiabático.
